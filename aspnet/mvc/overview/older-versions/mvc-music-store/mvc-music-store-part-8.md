---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-8
title: 'Parte 8: El carro con las actualizaciones de Ajax de la | Microsoft Docs'
author: jongalloway
description: Esta serie de tutoriales detalla todos los pasos realizados para compilar la aplicación de ejemplo de Music Store de ASP.NET MVC. Parte 8 cubre el carro de la compra con las actualizaciones de Ajax.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 26b2f55e-ed42-4277-89b0-c941eb754145
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-8
msc.type: authoredcontent
ms.openlocfilehash: 327b7ee4e302188323c229c231ae750cbed709a1
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37369801"
---
<a name="part-8-shopping-cart-with-ajax-updates"></a>Parte 8: Carro de la compra con las actualizaciones de Ajax
====================
por [Jon Galloway](https://github.com/jongalloway)

> El Store de música de MVC es una aplicación de tutorial que presenta y explica paso a paso cómo usar ASP.NET MVC y Visual Studio para el desarrollo web.  
>   
> El Store de música de MVC es una implementación de almacén de ejemplo ligera que vende álbumes de música en línea e implementa la administración básica del sitio, inicio de sesión de usuario y funcionalidad del carro de la compra.  
>   
> Esta serie de tutoriales detalla todos los pasos realizados para compilar la aplicación de ejemplo de Music Store de ASP.NET MVC. Parte 8 cubre el carro de la compra con las actualizaciones de Ajax.


Permitiremos que los usuarios que coloquen álbumes en su carro de compra sin registrar, pero debe registrar como invitados desproteger completa. El proceso de la compra y la desprotección se dividirá en dos controladores: un controlador de ShoppingCart que permite anónimamente agregar elementos a un carro de compra y un controlador de desprotección que controla el proceso de pago. Se deberá comenzar con el carro de la compra en esta sección y luego crear el proceso de pago en la sección siguiente.

## <a name="adding-the-cart-order-and-orderdetail-model-classes"></a>Agregar las clases de modelo del carro de compra, pedido y OrderDetail

Nuestros procesos de carro de la compra y la desprotección hará que el uso de las clases nuevas. Haga clic en la carpeta Models y agregue una clase de carro (Cart.cs) con el código siguiente.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample1.cs)]

Esta clase es bastante similar a otros usuarios que hemos usado hasta ahora, excepto el atributo [Key] para la propiedad de identificador de registro. Nuestros artículos del carro de compra tendrá un identificador de cadena denominado CartID para permitir compras anónimas, pero la tabla incluye una clave principal de entero denominada identificador de registro. Por convención, Code First de Entity Framework espera que la clave principal para una tabla denominada carro será CartId o Id., pero nos podemos reemplazar fácilmente mediante las anotaciones o código si queremos. Este es un ejemplo de cómo podemos usar las convenciones simples en Entity Framework Code First cuando sean adecuados para nosotros, pero, no estamos limitados por ellos cuando no lo hacen.

A continuación, agregue una clase Order (Order.cs) con el código siguiente.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample2.cs)]

Esta clase realiza el seguimiento de información de resumen y de entrega para un pedido. **No se compilará aún**, porque no tiene una propiedad de navegación OrderDetails que depende de una clase que no hemos creado aún. Vamos a corregir que ahora mediante la adición de una clase denominada OrderDetail.cs, agregue el código siguiente.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample3.cs)]

Nos aseguraremos de hacer una última actualización a nuestra clase MusicStoreEntities incluir DbSets que exponen las nuevas clases de modelo, también incluye una clase DbSet&lt;artista&gt;. La clase MusicStoreEntities actualizada aparece como a continuación.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample4.cs)]

## <a name="managing-the-shopping-cart-business-logic"></a>Administrar la lógica de negocios del carro de la compra

A continuación, vamos a crear la clase ShoppingCart en la carpeta Models. El modelo ShoppingCart controla el acceso a datos en la tabla de carro. Además, controlará la lógica de negocios a para agregar y quitar elementos del carro de compra.

Puesto que no desea exigir que los usuarios registrarse para obtener una cuenta agregar elementos al carro de la compra, se asignarán a los usuarios un identificador único temporal (con un GUID o el identificador único global) al tener acceso a la cesta. Almacenaremos este identificador mediante la clase de sesión de ASP.NET.

*Nota: La sesión de ASP.NET es un lugar conveniente para almacenar información específica del usuario que expirará después de que abandonen el sitio. Mientras uso indebido del estado de sesión puede tener implicaciones de rendimiento en los sitios más grandes, el uso de la luz funcionará bien para fines de demostración.*

La clase ShoppingCart expone los métodos siguientes:

**AddToCart** toma un álbum como parámetro y lo agrega al carro de compra del usuario. Puesto que realiza el seguimiento de la tabla del carro de la cantidad para cada álbum, incluye lógica para crear una nueva fila si es necesario o sólo incremente la cantidad, si el usuario ya ha solicitado una copia del álbum.

**RemoveFromCart** toma un identificador de álbum y lo quita del carro de compra del usuario. Si el usuario sólo tuviera una copia del álbum en su carro de compra, se quita la fila.

**EmptyCart** quita todos los elementos del carro de compra de un usuario.

**GetCartItems** recupera una lista de CartItems de visualización o procesamiento.

**GetCount** recupera un número total de álbumes de un usuario tiene en su carro de la compra.

**GetTotal** calcula el costo total de todos los artículos del carro.

**CreateOrder** convierte el carro de la compra a un pedido durante la fase de retirada.

**GetCart** es un método estático que permite a los nuestros controladores obtener un objeto de carro. Usa el **GetCartId** método para controlar el CartId de lectura de la sesión del usuario. El método GetCartId requiere el objeto HttpContextBase para que pueda leer CartId del usuario de la sesión del usuario.

Aquí es el conjunto completo **clase ShoppingCart**:

[!code-csharp[Main](mvc-music-store-part-8/samples/sample5.cs)]

## <a name="viewmodels"></a>ViewModels

Nuestro controlador de carro de la compra deberá comunicar cierta información a sus vistas compleja que no se asigna correctamente a nuestros objetos de modelo. No queremos modificar los modelos para que se ajuste a nuestras vistas; Clases de modelo deberían representar nuestro dominio, no la interfaz de usuario. Una solución sería pasar la información a nuestras vistas mediante la clase ViewBag, como hicimos con la información de la lista desplegable de Store Manager, pero pasando una gran cantidad de información a través de ViewBag obtiene difícil de administrar.

Una solución para este problema consiste en usar el *ViewModel* patrón. Al utilizar este patrón se creación clases fuertemente tipadas que están optimizados para escenarios de nuestra vista específica y que exponen propiedades para el valores o contenido dinámico necesita nuestras plantillas de vista. Nuestras clases de controlador, a continuación, pueden rellenar y pasar estas clases optimizadas para la vista a nuestra plantilla de vista para usar. Esto permite la seguridad de tipos, la comprobación en tiempo de compilación y el editor IntelliSense dentro de las plantillas de vista.

Vamos a crear dos modelos de vista para su uso en el controlador de carro de la compra: el ShoppingCartViewModel contendrá el contenido del carro de la compra del usuario y la ShoppingCartRemoveViewModel se usará para mostrar información de confirmación cuando un usuario quita algo en su carro de compra.

Vamos a crear una nueva carpeta ViewModels en la raíz de nuestro proyecto para que todo organizado. Haga clic en el proyecto, selecciono Agregar / nueva carpeta.

![](mvc-music-store-part-8/_static/image1.jpg)

Nombre de la carpeta ViewModels.

![](mvc-music-store-part-8/_static/image1.png)

A continuación, agregue la clase ShoppingCartViewModel en la carpeta ViewModels. Tiene dos propiedades: una lista de elementos del carro y un valor decimal para contener el precio total para todos los artículos del carro de compra.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample6.cs)]

Ahora, agregue el ShoppingCartRemoveViewModel a la carpeta ViewModels, con las siguientes cuatro propiedades.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample7.cs)]

## <a name="the-shopping-cart-controller"></a>El controlador de carro de la compra

El controlador de carro de la compra tiene tres objetivos principales: agregar elementos a un carro de compra, quitar elementos del carro de la y ver los elementos del carro de compra. Hace uso de las tres clases acaba de crear: ShoppingCartViewModel, ShoppingCartRemoveViewModel y ShoppingCart. Como se muestra en el StoreController y StoreManagerController, vamos a agregar un campo para contener una instancia de MusicStoreEntities.

Agregar un controlador de carro de la compra de nuevo al proyecto mediante la plantilla de controlador en blanco.

![](mvc-music-store-part-8/_static/image2.png)

Aquí es el controlador de ShoppingCart completa. Las acciones de índice y Agregar controlador resultará muy familiar. Las acciones de controlador Remove y CartSummary controlan dos casos especiales que explicaremos en la sección siguiente.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample8.cs)]

## <a name="ajax-updates-with-jquery"></a>Actualizaciones de AJAX con jQuery

A continuación, vamos a crear una página de índice de carro de la compra que está fuertemente tipada para el ShoppingCartViewModel y usa la plantilla de vista de lista con el mismo método que antes.

![](mvc-music-store-part-8/_static/image3.png)

Sin embargo, en lugar de usar un Html.ActionLink para quitar elementos del carro de la, vamos a usar jQuery para "conectar" el evento click para todos los vínculos en esta vista que tiene la clase RemoveLink HTML. En lugar de publicar el formulario, este controlador de eventos click simplemente realizará una devolución de llamada de AJAX en la acción de controlador RemoveFromCart. El RemoveFromCart devuelve un resultado JSON serializada, que la devolución de llamada de jQuery, a continuación, analiza y realiza cuatro actualizaciones rápidas a la página mediante jQuery:

- 1. Quita el álbum eliminado de la lista
- 2. Actualiza el recuento de carro en el encabezado
- 3. Muestra un mensaje de actualización para el usuario
- 4. Actualiza el precio total del carro de compra

Dado que el escenario remove se controlan mediante una devolución de llamada de Ajax en la vista de índice, no necesitamos una vista adicional para la acción RemoveFromCart. Este es el código completo de la vista /ShoppingCart/Index:

[!code-cshtml[Main](mvc-music-store-part-8/samples/sample9.cshtml)]

Para probarlo, es necesario poder agregar elementos a nuestro carro de compra. Actualizaremos nuestra **Store detalles** vista e incluir un botón "Agregar al carro". Mientras estamos en ello, podemos incluir parte de la información adicional de álbum que hemos agregado desde que se actualizó por última vez esta vista: género, intérprete, precio y carátulas de álbum. El código actualizado de vista de detalles de Store aparece como se muestra a continuación.

[!code-cshtml[Main](mvc-music-store-part-8/samples/sample10.cshtml)]

Ahora podemos hacer clic en el almacén y probar agregando y quitando álbumes hacia y desde nuestro carro de compra. Ejecute la aplicación y busque el índice de Store.

![](mvc-music-store-part-8/_static/image4.png)

A continuación, haga clic en un género para ver una lista de álbumes.

![](mvc-music-store-part-8/_static/image5.png)

Al hacer clic en un título de álbum ahora muestra nuestra vista Detalles del álbum actualizado, incluido el botón "Agregar al carro".

![](mvc-music-store-part-8/_static/image6.png)

Al hacer clic en el botón "Agregar al carro" muestra nuestra vista de índice de carro de la compra con la lista de resumen de carro de la compra.

![](mvc-music-store-part-8/_static/image7.png)

Después de cargar su carro de la compra, puede hacer clic en quitar el desde el vínculo del carro de compra para ver la actualización de Ajax al carro de la compra.

![](mvc-music-store-part-8/_static/image8.png)

Hemos creado un trabajo carro que permite a los usuarios no registrados agregar elementos al carro de la compra. En la sección siguiente, se permiten para registrar y realizar el proceso de pago.


> [!div class="step-by-step"]
> [Anterior](mvc-music-store-part-7.md)
> [Siguiente](mvc-music-store-part-9.md)
