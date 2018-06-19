---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
title: '-Parte 5: Lógica de negocios | Documentos de Microsoft'
author: JoeStagner
description: Esta serie de tutoriales detalla todos los pasos realizados para compilar la aplicación de ejemplo de Tailspin Spyworks. Parte 5 agrega alguna lógica de negocios.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: eaef475a-ca91-47ea-a4a7-d074005ed80c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
msc.type: authoredcontent
ms.openlocfilehash: e4342e634ef8c4bcf4e0085650a28f414ab23736
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30885086"
---
<a name="part-5-business-logic"></a>-Parte 5: Lógica de negocios
====================
por [Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks muestra cómo extraordinariamente simple es crear aplicaciones eficaces y escalables para la plataforma. NET. Muestra cómo usar las características nuevas en ASP.NET 4 para crear una tienda en línea, incluidos los de la compra, la desprotección y la administración.
> 
> Esta serie de tutoriales detalla todos los pasos realizados para compilar la aplicación de ejemplo de Tailspin Spyworks. Parte 5 agrega alguna lógica de negocios.


## <a id="_Toc260221671"></a>  Agregar alguna lógica de negocios

Deseamos que nuestra experiencia compra esté disponible cada vez que alguien visite nuestro sitio web. Los visitantes podrán examinar y agregar elementos al carro de la compra, incluso si no están registrados o registrados en. Cuando estén listas desproteger se le dará la opción de autenticar y si no lo son todavía los miembros podrán crear una cuenta.

Esto significa que tendrá que implementar la lógica para convertir el carro de la compra de un estado anónimo a un estado de "Usuario registrado".

Vamos a crear un directorio denominado "Clases", a continuación, haga doble clic en la carpeta y cree un nuevo archivo de "Clase" denominado MyShoppingCart.cs

![](tailspin-spyworks-part-5/_static/image1.jpg)

![](tailspin-spyworks-part-5/_static/image1.png)

Como se mencionó anteriormente se tendrá que ampliar la clase que implementa la página de MyShoppingCart.aspx y se encargará de hacerlo con. Construcción de "Clase parcial" eficaz de NET.

La llamada para nuestro archivo MyShoppingCart.aspx.cf generada tiene el siguiente aspecto.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample1.cs)]

Tenga en cuenta el uso de la palabra clave "partial".

El archivo de clase que se acaba de generar el siguiente aspecto.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample2.cs)]

Agregando la palabra clave partial a este archivo también se combinará nuestras implementaciones.

Nuestro archivo de clase nuevo ahora este aspecto.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample3.cs)]

El primer método que se agregará a la clase es el método de "AddItem". Este es el método que se llamará en última instancia cuando el usuario hace clic en los vínculos de "Agregar a imágenes" en las páginas de la lista de productos y detalles del producto.

Anexe lo siguiente para el uso de instrucciones en la parte superior de la página.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample4.cs)]

Y agregue este método a la clase MyShoppingCart.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample5.cs)]

Estamos utilizando LINQ to Entities para ver si el elemento ya está en el carro. Si por lo tanto, se actualiza la cantidad del pedido del elemento, en caso contrario, se crea una nueva entrada para el elemento seleccionado

Para poder llamar a este método se implementará una página AddToCart.aspx que no solo este método de la clase pero, a continuación, muestra un carro = de la compra cuando se ha agregado el elemento actual.

Haga doble clic en el nombre de la solución en el Explorador de soluciones y agregue y nueva página denominada AddToCart.aspx como lo hemos hecho previamente.

Aunque podríamos usar esta página para mostrar los resultados intermedios como problemas de cotizaciones bajos, etcetera, en nuestra implementación, la página se realmente no representan, pero en su lugar llame a la lógica de "Agregar" y redirigir.

Para ello, vamos a agregar el código siguiente a la página\_evento Load.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample6.cs)]

Tenga en cuenta que estamos recuperando el producto que desea agregar al carro de la compra de un parámetro de cadena de consulta y llamar al método AddItem de nuestra clase.

Suponiendo que no hay errores se encuentran el control pasa a la página de SHoppingCart.aspx que totalmente implementaremos a continuación. Si es necesario efectuar un error que se produzca una excepción.

Actualmente nos hemos no ha implementado un controlador de errores global para esta excepción quedarían no controlada por nuestra aplicación, pero se solucionará esto en breve.

Tenga en cuenta también el uso de la instrucción Debug.Fail() (disponible a través de `using System.Diagnostics;)`

Es la aplicación se ejecuta en el depurador, este método mostrará un cuadro de diálogo detallado con información sobre el estado de las aplicaciones junto con el mensaje de error que se especifique.

Cuando se ejecuta en producción el Debug.Fail() se omite la instrucción.

Puede observar en el código por encima de una llamada a un método en nuestros nombres de clase de carro de la compra "GetShoppingCartId".

Agregue el código para implementar el método como se indica a continuación.

Tenga en cuenta que también hemos agregado botones Actualizar y desprotección y una etiqueta donde podemos mostrar el carro de la "total".

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample7.cs)]

Ahora podemos agregar elementos a nuestro carro de la compra, pero no hemos implementado la lógica para mostrar el carro después de que se ha agregado un producto.

Por lo tanto, en la página MyShoppingCart.aspx vamos a agregar un control EntityDataSource y GridVire como se indica a continuación.

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample8.aspx)]

Llame el formulario en el diseñador para que pueda haga doble clic en el botón actualizar el carro y generar el controlador de eventos click que se especifica en la declaración en el marcado.

Implementaremos los detalles más adelante, pero hacerlo Permítanos compilará y ejecutará nuestra aplicación sin errores.

Cuando se ejecuta la aplicación y agregar un elemento al carro de la compra se verá lo siguiente.

![](tailspin-spyworks-part-5/_static/image2.jpg)

Tenga en cuenta que nos hemos desviado de la presentación de la cuadrícula de "default" mediante la implementación de tres columnas personalizadas.

El primero es un Editable, el campo "Enlace" para la cantidad:

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample9.aspx)]

La siguiente es una columna "calculada" que muestra el elemento de línea total (el elemento de coste multiplicado por la cantidad para poder ordenarse):

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample10.aspx)]

Por último, tenemos una columna personalizada que contiene un control de casilla de verificación que el usuario va a emplear para indicar que el elemento debe quitarse del plan de compra.

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample11.aspx)]

![](tailspin-spyworks-part-5/_static/image3.jpg)

Como puede ver, el orden Total de líneas está vacía, así que vamos a agregar lógica para calcular el Total del pedido.

Implementaremos en primer lugar un método de "GetTotal" a nuestra clase MyShoppingCart.

En el archivo MyShoppingCart.cs agregue el código siguiente.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample12.cs)]

A continuación, en la página\_controlador de eventos de carga se le puede llamar a nuestro método GetTotal. Al mismo tiempo, vamos a agregar una prueba para comprobar si el carro de la compra está vacío y ajuste la presentación en consecuencia, si.

Ahora si el carro de la compra está vacío nos enfrentamos a esto:

![](tailspin-spyworks-part-5/_static/image4.jpg)

Y si no es así, se muestra el total.

![](tailspin-spyworks-part-5/_static/image5.jpg)

Sin embargo, esta página no está completa.

Necesitamos lógica adicional para volver a calcular el carro de la compra mediante la eliminación de elementos marcados para su eliminación y mediante la determinación de nuevos valores de cantidad como algunos pueden se han cambiado en la cuadrícula por el usuario.

Permite agregar un método de "RemoveItem" a nuestra clase de carro de la compra en MyShoppingCart.cs para controlar el caso cuando un usuario marca un elemento para su eliminación.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample13.cs)]

Ahora vamos a ad un método para controlar las circunstancias, cuando un usuario simplemente cambia la calidad se ordenen en GridView.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample14.cs)]

Podemos implementar la lógica que realmente actualiza el carro de la compra en la base de datos con las características básicas de quitar y actualizar en su lugar. (En MyShoppingCart.cs)

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample15.cs)]

Se deberá tener en cuenta que este método espera dos parámetros. Uno es la cesta de identificador y el otro es una matriz de objetos de tipo definido por el usuario.

Con el fin de minimizar la dependencia de nuestra lógica sobre los detalles de la interfaz de usuario, hemos definido una estructura de datos que podemos usar para pasar los elementos del carro de la compra a nuestro código sin el método que necesitan tener acceso directamente el control GridView.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample16.cs)]

En nuestro archivo MyShoppingCart.aspx.cs podemos usar esta estructura en el controlador de evento Click de botón de actualización como se indica a continuación. Tenga en cuenta que además de actualizar el carro actualizaremos también el total de carro.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample17.cs)]

Tenga en cuenta con un interés particular esta línea de código:

[!code-javascript[Main](tailspin-spyworks-part-5/samples/sample18.js)]

GetValues() es una función auxiliar especial que se implementará en MyShoppingCart.aspx.cs como se indica a continuación.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample19.cs)]

Esto proporciona una manera limpia para tener acceso a los valores de los elementos de enlace de nuestro control GridView. Puesto que nuestro Control de casilla de verificación "Quitar el elemento" no está enlazado se podrá acceder a él a través del método FindControl().

En esta fase de desarrollo del proyecto ahora obtenemos está listos para implementar el proceso de pago.

Antes de que al hacerlo, vamos a usar Visual Studio para generar la base de datos de pertenencia y agregar un usuario en el repositorio de pertenencia.

> [!div class="step-by-step"]
> [Anterior](tailspin-spyworks-part-4.md)
> [Siguiente](tailspin-spyworks-part-6.md)
