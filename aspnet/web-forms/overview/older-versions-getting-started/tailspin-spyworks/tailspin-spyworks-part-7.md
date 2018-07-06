---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
title: 'Parte 7: Incorporación de características | Microsoft Docs'
author: JoeStagner
description: Esta serie de tutoriales detalla todos los pasos que se tarda en compilar la aplicación de ejemplo Tailspin Spyworks. Parte 7 agrega características adicionales, como revisa cuenta...
ms.author: aspnetcontent
ms.date: 07/21/2010
ms.assetid: 50223ee9-11b9-4cf3-bca2-e2f10bf471f3
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
msc.type: authoredcontent
ms.openlocfilehash: 47402ccdfb702dc1bb1bdb4e634a7cd6f5ebc235
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37831653"
---
<a name="part-7-adding-features"></a>Parte 7: Agregar características
====================
por [Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks demuestra cómo extraordinariamente simple es crear aplicaciones eficaces y escalables para la plataforma. NET. Resalta cómo usar las características nuevas en ASP.NET 4 para crear una tienda en línea, incluida la compra, la desprotección y la administración.
> 
> Esta serie de tutoriales detalla todos los pasos que se tarda en compilar la aplicación de ejemplo Tailspin Spyworks. Parte 7 agrega características adicionales, como la revisión de la cuenta, reseñas de productos y "elementos populares" y los controles de usuario "también adquiridas".


## <a id="_Toc260221673"></a>  Agregar características

Aunque los usuarios pueden examinar nuestro catálogo, colocar elementos en su carro de la compra y completar el proceso de formalización, hay que un número de admitir las características que se incluirá para mejorar nuestro sitio.

1. Revisión de la cuenta (lista de pedidos colocaron y ver los detalles.)
2. Agregar algún contenido específico de contexto a la página inicial.
3. Agregar una característica para permitir que los usuarios de revisión de los productos en el catálogo.
4. Crear un Control de usuario para mostrar elementos populares y contexto que controlan en la página principal.
5. Crear un control de usuario "También adquirido" y agregarlo a la página de detalles del producto.
6. Agregar un contacto de página.
7. Agregar una página.
8. Error global

## <a id="_Toc260221674"></a>  Revisión de la cuenta

En la carpeta de "Cuenta", cree dos páginas .aspx uno OrderList.aspx con nombre y el otro OrderDetails.aspx con nombre

OrderList.aspx aprovechará los controles GridView y EntityDataSoure mucho que tenemos anteriormente.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample1.aspx)]

El EntityDataSoure selecciona los registros de la tabla de pedidos filtrada por el nombre de usuario (consulte la WhereParameter) que se establece en una variable de sesión cuando el registro de usuario.

Tenga en cuenta también estos parámetros en el campo Hyperlink del control GridView:

[!code-xml[Main](tailspin-spyworks-part-7/samples/sample2.xml)]

Estos especifican que el vínculo a la vista de detalles de pedido para cada producto especificando el campo OrderID como un parámetro de cadena de consulta a la página OrderDetails.aspx.

## <a id="_Toc260221675"></a>  OrderDetails.aspx

Usaremos un control EntityDataSource para tener acceso a los pedidos y un FormView para mostrar los datos del pedido y otro EntityDataSource con un control GridView para mostrar los elementos de línea de todos los pedidos.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample3.aspx)]

En el archivo (OrderDetails.aspx.cs) de código subyacente, tenemos dos bits poco ciertos aspectos de mantenimiento.

En primer lugar, debe asegurarse de que OrderDetails siempre obtiene un OrderId.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample4.cs)]

También es necesario calcular y mostrar el total de los elementos de línea del pedido.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample5.cs)]

## <a id="_Toc260221676"></a>  La página principal

Vamos a agregar algún contenido estático a la página Default.aspx.

Primero crearé una carpeta de "Contenido" y dentro de él una carpeta de imágenes (y deberá incluir una imagen que se usará en la página principal).

En el marcador de posición de la parte inferior de la página Default.aspx, agregue el marcado siguiente.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample6.aspx)]

## <a id="_Toc260221677"></a>  Reseñas de productos

En primer lugar, vamos a agregar un botón con un vínculo a un formulario que podemos usar para escribir una reseña del producto.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample7.aspx)]

![](tailspin-spyworks-part-7/_static/image1.jpg)

Tenga en cuenta que el ProductID que estamos pasando la cadena de consulta

Siguiente vamos a agregar página denominada ReviewAdd.aspx

Esta página utilizará ASP.NET AJAX Control Toolkit. Si aún no ha hecho por lo que puede descargar desde [DevExpress](http://devexpress.com/act) y hay una guía sobre cómo configurar el Kit de herramientas para su uso con Visual Studio aquí [ https://www.asp.net/learn/ajax-videos/video-76.aspx ](../../../videos/ajax-control-toolkit/how-do-i-get-started-with-the-aspnet-ajax-control-toolkit.md).

En el modo de diseño, arrastre los controles y controles de validación en el cuadro de herramientas y crear un formulario como la siguiente.

![](tailspin-spyworks-part-7/_static/image2.jpg)

El marcado tendrá un aspecto algo parecido a esto.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample8.aspx)]

Ahora que podemos escribir las revisiones, permite mostrar las revisiones en la página del producto.

Este marcado se agregue a la página ProductDetails.aspx.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample9.aspx)]

Ejecute ahora nuestra aplicación y navegar a un producto muestran la información del producto incluidas las revisiones de cliente.

![](tailspin-spyworks-part-7/_static/image3.jpg)

## <a id="_Toc260221678"></a>  Control de elementos más populares (creación de controles de usuario)

Con el fin de aumentar las ventas en el sitio web, se agregará un par de características a los productos de "venta sugeridos" populares o relacionados.

La primera de ellas será una lista de los productos más populares en nuestro catálogo de productos.

Crearemos un "Control de usuario" para mostrar los elementos en la página principal de nuestra aplicación más vendidos. Puesto que se trata de un control, nos podemos usar en cualquier página simplemente arrastrando y colocando el control en el Diseñador de Visual Studio en cualquier página que nos gusta.

En el Explorador de soluciones de Visual Studio, haga doble clic en el nombre de la solución y cree un nuevo directorio denominado "Controles". Aunque no es necesario hacerlo, nos ayudará a mantener nuestro proyecto organizada creando nuestro todos los controles de usuario en el directorio "Controles".

Haga doble clic en la carpeta controls y elija "Nuevo elemento":

![](tailspin-spyworks-part-7/_static/image4.jpg)

Especifique un nombre para el control de "PopularItems". Tenga en cuenta que la extensión de archivo para los controles de usuario ascx .aspx no.

El control de usuario de los elementos más populares se definirá como sigue.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample10.aspx)]

Vamos a usar un método que no hemos usado aún en esta aplicación. Estamos usando el control repeater y en lugar de usar un control de origen de datos enlazamos el Control Repeater a los resultados de una consulta LINQ to Entities.

En el código subyacente de nuestro control hacerlo como sigue.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample11.cs)]

Tenga en cuenta también esta línea importante en la parte superior del marcado de nuestro control.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample12.aspx)]

Puesto que no cambiará los elementos más populares minuto a minuto, podemos agregar una directiva dolor para mejorar el rendimiento de nuestra aplicación. Esta directiva hará que el código de los controles que solo se ejecutará cuando expira la salida almacenada en caché del control. En caso contrario, se usará la versión en caché de salida del control.

Ahora sólo tenemos que hacer es incluir el control de nuevo en nuestra página Default.aspc.

Utilice arrastrar y colocar para colocar una instancia del control en la columna abierta el formulario predeterminado.

![](tailspin-spyworks-part-7/_static/image5.jpg)

Ahora cuando ejecutamos nuestra aplicación de la página principal muestran los elementos más populares.

![](tailspin-spyworks-part-7/_static/image6.jpg)

## <a id="_Toc260221679"></a>  "También adquirido" controlar (controles de usuario con parámetros)

El segundo Control de usuario que vamos a crear tendrá sugerido de venta al siguiente nivel mediante la adición de especificidad de contexto.

La lógica para calcular los elementos superiores "También adquirido" no es trivial.

Nuestro control "También adquirido" seleccionar los registros de OrderDetails (comprados anteriormente) para el ProductID seleccionado actualmente y captar los OrderID para cada pedido único que se encuentra.

A continuación, vamos a seleccionar a los productos de todos los pedidos y suma las cantidades adquiridas. Comenzaremos ordenar los productos que suman una cantidad y mostrar los elementos de los cinco principales.

Dada la complejidad de esta lógica, se implementará este algoritmo como un procedimiento almacenado.

La instrucción T-SQL para el procedimiento almacenado es como sigue.

[!code-sql[Main](tailspin-spyworks-part-7/samples/sample13.sql)]

Tenga en cuenta que este procedimiento almacenado (SelectPurchasedWithProducts) existía en la base de datos cuando se incluyen en nuestra aplicación y cuando se genera el Entity Data Model se especifica que, además de las tablas y vistas que debíamos, Entity Data Model debe incluir este procedimiento almacenado.

Para tener acceso al procedimiento almacenado de Entity Data Model se debe importar la función.

Haga doble clic en el Entity Data Model en el Explorador de soluciones para abrirlo en el diseñador y abra el Explorador de modelos, a continuación, haga clic en el diseñador y seleccione "Agregar importación de función".

![](tailspin-spyworks-part-7/_static/image1.png)

Si lo hace, se abrirá este cuadro de diálogo.

![](tailspin-spyworks-part-7/_static/image2.png)

Rellene los campos como observar más arriba, seleccionando la "SelectPurchasedWithProducts" y use el nombre del procedimiento para el nombre de la función importada.

Haga clic en "Aceptar".

Hecho esto que podemos simplemente programaremos con el procedimiento almacenado como se podría cualquier otro elemento en el modelo.

Por lo tanto, en la carpeta "Controles" crear un nuevo control de usuario denominado AlsoPurchased.ascx.

El marcado para este control le parecerán conocido para el control PopularItems.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample14.aspx)]

La diferencia notable es que son no almacena en caché la salida desde que se va a representar el elemento variarán por producto.

El ProductId será "property" para el control.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample15.cs)]

En el controlador de evento PreRender del control se eed hacer tres cosas.

1. Asegúrese de que se ha establecido el ProductID.
2. Consulte si hay productos que se han adquirido con la actual.
3. Algunos elementos, como se determina en #2 de salida.

Tenga en cuenta lo fácil que es llamar al procedimiento almacenado mediante el modelo.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample16.cs)]

Después de determinar que no existe "también adquieren" simplemente podemos enlazar el control repeater a los resultados devueltos por la consulta.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample17.cs)]

Si no hubiera ningún elemento "también adquirido" simplemente mostraremos otros elementos más populares de nuestro catálogo.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample18.cs)]

Para ver los elementos "También comprar", abra la página ProductDetails.aspx y arrastre el control AlsoPurchased desde el Explorador de soluciones para que aparezca en esta posición en el marcado.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample19.aspx)]

Si lo hace, creará una referencia al control en la parte superior de la página ProductDetails.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample20.aspx)]

Puesto que el control de usuario AlsoPurchased requiere un número de ProductId se establecerá la propiedad ProductID de nuestro control mediante el uso de una instrucción Eval en el elemento de modelo de datos actual de la página.

![](tailspin-spyworks-part-7/_static/image3.png)

Cuando nos compilar y ejecutar ahora y vaya a un producto vemos los elementos "Adquirido también".

![](tailspin-spyworks-part-7/_static/image7.jpg)

> [!div class="step-by-step"]
> [Anterior](tailspin-spyworks-part-6.md)
> [Siguiente](tailspin-spyworks-part-8.md)
