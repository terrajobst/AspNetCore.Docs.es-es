---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
title: 'Parte 7: Agregar características | Documentos de Microsoft'
author: JoeStagner
description: Esta serie de tutoriales detalla todos los pasos realizados para compilar la aplicación de ejemplo de Tailspin Spyworks. Parte 7 agregan características adicionales, como cuenta revisa...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 50223ee9-11b9-4cf3-bca2-e2f10bf471f3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
msc.type: authoredcontent
ms.openlocfilehash: 17f068155f6726047901e2f7d580d375a4e07c87
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="part-7-adding-features"></a>Parte 7: Agregar características
====================
por [Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks muestra cómo extraordinariamente simple es crear aplicaciones eficaces y escalables para la plataforma. NET. Muestra cómo usar las características nuevas en ASP.NET 4 para crear una tienda en línea, incluidos los de la compra, la desprotección y la administración.
> 
> Esta serie de tutoriales detalla todos los pasos realizados para compilar la aplicación de ejemplo de Tailspin Spyworks. Parte 7 agregan características adicionales, como la revisión de cuenta, las revisiones del producto y "elementos populares" y controles de usuario "también adquiridas".


## <a id="_Toc260221673"></a>  Agregar características

Aunque los usuarios pueden explorar nuestro catálogo, colocar elementos en su carro de la compra y completar el proceso de extracción del repositorio, hay que una serie de compatibilidad con características que se incluirá para mejorar nuestro sitio.

1. Revisión de cuenta (lista de pedidos colocaron y ver los detalles.)
2. Agregar algún contenido específico de contexto a la página principal.
3. Agregar una característica para que los usuarios puedan revisar los productos en el catálogo.
4. Crear un Control de usuario para mostrar elementos populares y contexto que controlan en la página principal.
5. Crear un control de usuario "También adquirido" y agregarla a la página de detalles del producto.
6. Agregar un contacto de página.
7. Agregar una página.
8. Error global

## <a id="_Toc260221674"></a>  Revisión de cuenta

En la carpeta de "Cuenta", cree dos páginas .aspx una OrderList.aspx con nombre y el otro OrderDetails.aspx con nombre

OrderList.aspx aprovecharán los controles GridView y EntityDataSoure hasta tenemos previamente.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample1.aspx)]

El EntityDataSoure selecciona los registros de la tabla Orders filtrada en el nombre de usuario (vea el WhereParameter) que se establece en una variable de sesión cuando el inicio de sesión de usuario.

Tenga en cuenta también estos parámetros en el campo Hyperlink de GridView:

[!code-xml[Main](tailspin-spyworks-part-7/samples/sample2.xml)]

Estos especifican el vínculo a la vista de detalles de pedido para cada producto que se especifica el campo OrderID como un parámetro de cadena de consulta a la página OrderDetails.aspx.

## <a id="_Toc260221675"></a>  OrderDetails.aspx

Se usará un control EntityDataSource para tener acceso a los pedidos y una FormView para mostrar los datos del pedido y otro EntityDataSource con un control GridView para mostrar los elementos de línea de todos los pedidos.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample3.aspx)]

En el archivo (OrderDetails.aspx.cs) de código subyacente, tenemos dos bits poco de mantenimiento.

En primer lugar, se tiene que asegurarse de que OrderDetails siempre obtiene un OrderId.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample4.cs)]

También es necesario calcular y mostrar el total de los artículos de línea del pedido.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample5.cs)]

## <a id="_Toc260221676"></a>  La página principal

Vamos a agregar contenido estático a la página Default.aspx.

En primer lugar creará una carpeta de "Contenido" y contiene una carpeta de imágenes (y deberá incluir una imagen que se usará en la página principal).

En el marcador de posición de la parte inferior de la página Default.aspx, agregue el siguiente marcado.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample6.aspx)]

## <a id="_Toc260221677"></a>  Revisiones del producto

Primero vamos a agregar un botón con un vínculo a un formulario que podemos usar para escribir una reseña del producto.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample7.aspx)]

![](tailspin-spyworks-part-7/_static/image1.jpg)

Tenga en cuenta que pasamos el ProductID en la cadena de consulta

Siguiente vamos a agregar página denominada ReviewAdd.aspx

Esta página usará el Kit de herramientas de Control de AJAX de ASP.NET. Si aún no ha hecho por lo que puede descargar desde [DevExpress](http://devexpress.com/act) y hay una guía sobre cómo configurar el Kit de herramientas para su uso con Visual Studio aquí [ https://www.asp.net/learn/ajax-videos/video-76.aspx ](../../../videos/ajax-control-toolkit/how-do-i-get-started-with-the-aspnet-ajax-control-toolkit.md).

En modo de diseño, arrastre controles y controles de validación en el cuadro de herramientas y crear un formulario como el siguiente.

![](tailspin-spyworks-part-7/_static/image2.jpg)

El marcado tendrá un aspecto similar al siguiente.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample8.aspx)]

Ahora que podemos escribir revisiones, permite mostrar las revisiones en la página del producto.

Agregue este marcado para la página ProductDetails.aspx.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample9.aspx)]

Ejecuta la aplicación tiene ahora y navegar a un producto muestran la información del producto incluidas revisiones de los clientes.

![](tailspin-spyworks-part-7/_static/image3.jpg)

## <a id="_Toc260221678"></a>  Control de elementos populares (creación de controles de usuario)

Con el fin de aumentar las ventas en el sitio web se agregará un par de características a "ventas sugeridas" populares o relacionada con productos.

La primera de estas características será una lista de los productos más populares en nuestro catálogo de productos.

Se creará un "Control de usuario" para mostrar lo elementos en la página principal de la aplicación más vendido. Puesto que se trata de un control, se puede utilizar en cualquier página simplemente arrastrando y colocando el control en el Diseñador de Visual Studio en cualquier página que nos gusta.

En el Explorador de soluciones de Visual Studio, haga doble clic en el nombre de la solución y crear un nuevo directorio denominado "Controles". Aunque no es necesario hacerlo, se le ayudará a mantener nuestro proyecto organizado mediante la creación de los controles de usuario en el directorio "Controles".

Haga doble clic en la carpeta de controles y seleccione "Nuevo elemento":

![](tailspin-spyworks-part-7/_static/image4.jpg)

Especifique un nombre para el control de "PopularItems". Tenga en cuenta que la extensión de archivo para los controles de usuario es. ascx .aspx no.

Nuestro control de usuario populares de elementos se definirán como se indica a continuación.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample10.aspx)]

Aquí usamos un método que no hemos usado todavía en esta aplicación. Estamos usando el control de repetidor y en lugar de usar un control de origen de datos se estamos enlazando el Control de repetidor a los resultados de una consulta LINQ to Entities.

En el código subyacente de nuestro control hacemos como se indica a continuación.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample11.cs)]

Tenga en cuenta también esta línea importante en la parte superior del marcado de nuestro control.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample12.aspx)]

Puesto que no va a modificar los elementos más populares en forma de minuto a minuto podemos agregar una directiva dolor para mejorar el rendimiento de la aplicación. Esta directiva hará que el código de controles que solo se ejecuta cuando expira la salida almacenada en caché del control. En caso contrario, se usará la versión en caché de salida del control.

Ahora todo lo que queda por hacer es incluir el nuevo control en nuestra página de Default.aspc.

Usar arrastrar y colocar para colocar una instancia del control en la columna pendiente de nuestro formulario de manera predeterminada.

![](tailspin-spyworks-part-7/_static/image5.jpg)

Cuando se ejecuta nuestra aplicación, la página de inicio muestra ahora los elementos más populares.

![](tailspin-spyworks-part-7/_static/image6.jpg)

## <a id="_Toc260221679"></a>  "También adquirido" controlar (controles de usuario con parámetros)

El segundo Control de usuario que vamos a crear tendrá sugerido de venta al siguiente nivel mediante la adición de especificidad de contexto.

La lógica para calcular los elementos superiores "Adquirido también" no es trivial.

Nuestro control "También adquirido" seleccionará los registros de OrderDetails (anteriormente se vendía) para el identificador de producto seleccionado actualmente y agarre el OrderIDs para cada pedido único que se encuentra.

A continuación, configuraremos a los productos de todos los pedidos y suma las cantidades adquiridas. Comenzaremos ordenar los productos que suman una cantidad y mostrar los primeros cinco elementos.

Dada la complejidad de esta lógica, se implementará este algoritmo como un procedimiento almacenado.

El código T-SQL para el procedimiento almacenado es como sigue.

[!code-sql[Main](tailspin-spyworks-part-7/samples/sample13.sql)]

Tenga en cuenta que este procedimiento almacenado (SelectPurchasedWithProducts) existía en la base de datos cuando se incluye en nuestra aplicación, y cuando se genera el Entity Data Model se especifica que, además de las tablas y vistas que se necesitan, el Entity Data Model debe incluir este procedimiento almacenado.

Para tener acceso al procedimiento almacenado de Entity Data Model, es necesario importar la función.

Haga doble clic en el Entity Data Model en el Explorador de soluciones para abrirlo en el diseñador y abra el Explorador de modelos, a continuación, haga clic en el diseñador y seleccione "Agregar importación de funciones".

![](tailspin-spyworks-part-7/_static/image1.png)

Si lo hace, se abrirá este cuadro de diálogo.

![](tailspin-spyworks-part-7/_static/image2.png)

Rellene los campos como ver anteriormente, al seleccionar "SelectPurchasedWithProducts" y usar el nombre del procedimiento para el nombre de la función importada.

Haga clic en "Aceptar".

Hecho esto que simplemente se podemos programar en el procedimiento almacenado como se podría cualquier otro elemento en el modelo.

Por lo tanto, en la carpeta "Controles" crear un nuevo control de usuario denominado AlsoPurchased.ascx.

El marcado para este control le parecerán conocido para el control de PopularItems.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample14.aspx)]

La diferencia notable es que son no almacena en caché la salida desde que se va a representar el elemento se diferenciarán por producto.

El ProductId será "property" para el control.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample15.cs)]

En el controlador de evento PreRender del control se eed hacer tres cosas.

1. Asegúrese de que está establecido el ProductID.
2. Vea si hay productos que se han adquirido con la actual.
3. Algunos elementos como se determina en #2 de salida.

Tenga en cuenta lo fácil que es llamar al procedimiento almacenado a través del modelo.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample16.cs)]

Después de determinar que no existe "también adquieren" simplemente podemos enlazar repetidor a los resultados devueltos por la consulta.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample17.cs)]

Si no hubiera ningún elemento "también adquirido" simplemente mostraremos otros artículos populares de nuestro catálogo.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample18.cs)]

Para ver los elementos "También comprar", abra la página ProductDetails.aspx y arrastrar el control de AlsoPurchased desde el Explorador de soluciones para que aparezca en esta posición en el marcado.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample19.aspx)]

Si lo hace, creará una referencia al control en la parte superior de la página de ProductDetails.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample20.aspx)]

Debido a que el control de usuario AlsoPurchased requiere un número de ProductId se establecerá la propiedad ProductID de nuestro control mediante una instrucción Eval con respecto al elemento de modelo de datos actual de la página.

![](tailspin-spyworks-part-7/_static/image3.png)

Cuando se compilar y ejecutar ahora y busque un producto vemos los elementos "También comprar".

![](tailspin-spyworks-part-7/_static/image7.jpg)

> [!div class="step-by-step"]
> [Anterior](tailspin-spyworks-part-6.md)
> [Siguiente](tailspin-spyworks-part-8.md)
