---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
title: 'Parte 5: Lógica de negocios | Microsoft Docs'
author: JoeStagner
description: Esta serie de tutoriales detalla todos los pasos que se tarda en compilar la aplicación de ejemplo Tailspin Spyworks. Parte 5 agrega alguna lógica de negocios.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: eaef475a-ca91-47ea-a4a7-d074005ed80c
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
msc.type: authoredcontent
ms.openlocfilehash: 719d56e0764e2f66b8813c9487119bbc700d738c
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37377991"
---
<a name="part-5-business-logic"></a>Parte 5: Lógica de negocios
====================
por [Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks demuestra cómo extraordinariamente simple es crear aplicaciones eficaces y escalables para la plataforma. NET. Resalta cómo usar las características nuevas en ASP.NET 4 para crear una tienda en línea, incluida la compra, la desprotección y la administración.
> 
> Esta serie de tutoriales detalla todos los pasos que se tarda en compilar la aplicación de ejemplo Tailspin Spyworks. Parte 5 agrega alguna lógica de negocios.


## <a id="_Toc260221671"></a>  Adición de lógica de negocios

Queremos que nuestra experiencia de compra esté disponible cada vez que alguien visite nuestro sitio web. Los visitantes podrán examinar y agregar elementos al carro de la compra incluso si no están registrados ni ha iniciado sesión. Cuando estén listas desproteger se les dará la opción de autenticar y si no lo son aún los miembros podrán crear una cuenta.

Esto significa que debemos implementar la lógica para convertir el carro de la compra de un estado anónimo a un estado de "Usuario registrado".

Vamos a crear un directorio denominado "Clases", a continuación, haga doble clic en la carpeta y cree un nuevo archivo de "Class" denominado MyShoppingCart.cs

![](tailspin-spyworks-part-5/_static/image1.jpg)

![](tailspin-spyworks-part-5/_static/image1.png)

Como se mencionó anteriormente ampliaremos la clase que implementa la página MyShoppingCart.aspx y se realizará este paso mediante. Construcción de la red eficaz "clase parcial".

La llamada para nuestro archivo MyShoppingCart.aspx.cf generada tiene este aspecto.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample1.cs)]

Tenga en cuenta el uso de la palabra clave "parcial".

El archivo de clase que se acaba de generar tiene este aspecto.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample2.cs)]

Combinaremos nuestras implementaciones mediante la adición de la palabra clave parcial a este archivo también.

Nuestro nuevo archivo de clase ahora este aspecto.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample3.cs)]

El primer método que agregamos a nuestra clase es el método "AddItem". Este es el método que se llamará en última instancia, cuando el usuario hace clic en los vínculos de "Agregar a imágenes" en las páginas de lista de productos y los detalles del producto.

Agregue lo siguiente para el uso de instrucciones en la parte superior de la página.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample4.cs)]

Y agregue este método a la clase MyShoppingCart.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample5.cs)]

Estamos usando LINQ to Entities para ver si el elemento ya está en el carro de compra. Si por lo tanto, se actualiza la cantidad del pedido del elemento, en caso contrario, se creará una nueva entrada para el elemento seleccionado

Para poder llamar a este método se implementará una página AddToCart.aspx que no solo este método de clase, pero, a continuación, muestra un carro = de la compra después de que se ha agregado el elemento actual.

Haga doble clic en el nombre de la solución en el Explorador de soluciones y agregue y nueva página denominada AddToCart.aspx como se ha hecho anteriormente.

Aunque podríamos usamos esta página para mostrar los resultados provisionales como baja problemas stock, etcetera, en nuestra implementación, la página realmente no representan, pero en su lugar llamará a la lógica de "Agregar" y redirigir.

Para lograr esto, vamos a agregar el código siguiente a la página\_evento Load.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample6.cs)]

Tenga en cuenta que estamos recuperando el producto para agregar al carro de la compra de un parámetro de cadena de consulta y una llamada al método AddItem de nuestra clase.

Suponiendo que no hay errores se encuentran el control pasa a la página SHoppingCart.aspx que totalmente se implementará a continuación. Si debe haber un error se produce una excepción.

Actualmente, no hemos implementado todavía un controlador de errores global para esta excepción iría no controlada por nuestra aplicación, pero se solucionará esto en breve.

Tenga en cuenta también el uso de la instrucción Debug.Fail() (disponible a través de `using System.Diagnostics;)`

Es la aplicación se ejecuta en el depurador, este método mostrará un cuadro de diálogo detallada con información sobre el estado de las aplicaciones junto con el mensaje de error que se especifique.

Cuando se ejecuta en producción el Debug.Fail() se omite la instrucción.

Puede observar en el código anterior de una llamada a un método en nuestros nombres de clase de carro de la compra "GetShoppingCartId".

Agregue el código para implementar el método como sigue.

Tenga en cuenta que también hemos agregado los botones de actualización y la desprotección y una etiqueta donde podemos mostrar el carro de compra "total".

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample7.cs)]

Ahora podemos agregar elementos a nuestro carro de compra, pero no hemos implementado la lógica para mostrar el carro de compra una vez se ha agregado un producto.

Por lo tanto, en la página MyShoppingCart.aspx agregaremos un control EntityDataSource y un control GridVire como sigue.

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample8.aspx)]

Llame el formulario en el diseñador para que pueda hacer doble clic en el botón actualizar el carro y generar el controlador de eventos click que se especifica en la declaración en el marcado.

Implementaremos los detalles más adelante, pero hacerlo Permítanos compilará y ejecutará nuestra aplicación sin errores.

Cuando se ejecuta la aplicación y agregar un elemento al carro de la compra verá lo siguiente.

![](tailspin-spyworks-part-5/_static/image2.jpg)

Tenga en cuenta que se han desviado de la presentación de cuadrícula "default" mediante la implementación de tres columnas personalizadas.

El primero es un Editable, el campo "Enlace" para la cantidad de:

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample9.aspx)]

La siguiente es una columna "calculada" que muestra el elemento de línea total (el elemento de coste multiplicado por la cantidad se ordenen):

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample10.aspx)]

Por último, tenemos una columna personalizada que contiene un control de casilla de verificación que el usuario va a usar para indicar que el elemento debe quitarse del plan de compra.

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample11.aspx)]

![](tailspin-spyworks-part-5/_static/image3.jpg)

Como puede ver, el orden Total de líneas está vacío, así que vamos a agregar lógica para calcular el Total del pedido.

En primer lugar implementaremos un método "GetTotal" a nuestra clase MyShoppingCart.

En el archivo MyShoppingCart.cs agregue el código siguiente.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample12.cs)]

A continuación, en la página\_controlador de eventos de carga puede llamaremos a nuestro método GetTotal. Al mismo tiempo, vamos a agregar una prueba para comprobar si el carro de la compra está vacía y ajuste la presentación en consecuencia, si se trata.

Ahora si el carro de la compra está vacía, obtener esto:

![](tailspin-spyworks-part-5/_static/image4.jpg)

Y si no, vemos que nuestro total.

![](tailspin-spyworks-part-5/_static/image5.jpg)

Sin embargo, esta página no está completa.

Necesitamos una lógica adicional para volver a calcular el carro de la compra mediante la eliminación de los elementos marcados para su eliminación y determinando los nuevos valores de cantidad como algunos es posible que se han cambiado en la cuadrícula por el usuario.

Permite agregar un método "RemoveItem" a nuestra clase de carro de la en MyShoppingCart.cs para controlar el caso cuando un usuario marca un elemento para su eliminación.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample13.cs)]

Ahora vamos a ad un método para controlar el caso cuando un usuario simplemente cambia la calidad se ordenen en GridView.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample14.cs)]

Podemos implementar la lógica que realmente actualiza el carro de la compra en la base de datos con las características básicas de eliminación y actualización en su lugar. (En MyShoppingCart.cs)

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample15.cs)]

Observará que este método espera dos parámetros. Uno es el Id. de carro y el otro es una matriz de objetos de tipo definido por el usuario.

Con el fin de minimizar la dependencia de nuestra lógica sobre características específicas de interfaz de usuario, hemos definido una estructura de datos que podemos usar para pasar los elementos del carro de la compra a nuestro código sin nuestro método necesite acceder directamente el control GridView.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample16.cs)]

En nuestro archivo MyShoppingCart.aspx.cs podemos usar esta estructura en el controlador de evento Click de botón de actualización como se indica a continuación. Tenga en cuenta que además de actualizar el carro de compra se actualizará también el total del carro de compra.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample17.cs)]

Tenga en cuenta con un interés particular esta línea de código:

[!code-javascript[Main](tailspin-spyworks-part-5/samples/sample18.js)]

GetValues() es una función auxiliar especial que se implementará en MyShoppingCart.aspx.cs como sigue.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample19.cs)]

Esto proporciona una manera limpia para tener acceso a los valores de los elementos enlazados de nuestro control GridView. Puesto que no está enlazado el Control de casilla de verificación "Quitar el elemento" se tendrá acceso a él a través del método FindControl().

En esta fase de desarrollo del proyecto estamos preparados implementar el proceso de pago.

Antes de que al hacerlo, vamos a usar Visual Studio para generar la base de datos de pertenencia y agregar un usuario en el repositorio de pertenencia.

> [!div class="step-by-step"]
> [Anterior](tailspin-spyworks-part-4.md)
> [Siguiente](tailspin-spyworks-part-6.md)
