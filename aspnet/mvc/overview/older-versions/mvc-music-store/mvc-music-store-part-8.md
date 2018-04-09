---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-8
title: 'Parte 8: Carrito con las actualizaciones de Ajax | Documentos de Microsoft'
author: jongalloway
description: Esta serie de tutoriales detalla todos los pasos realizados para compilar la aplicación de ejemplo de la tienda de música de MVC de ASP.NET. Parte 8 cubre el carro de la compra con las actualizaciones de Ajax.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 26b2f55e-ed42-4277-89b0-c941eb754145
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-8
msc.type: authoredcontent
ms.openlocfilehash: 195c01ff0d71b2bfd0c00e71244d47a166330921
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="part-8-shopping-cart-with-ajax-updates"></a>Parte 8: Carro de la compra con las actualizaciones de Ajax
====================
por [Jon Galloway](https://github.com/jongalloway)

> La tienda de música de MVC es una aplicación de tutorial que presenta y explica paso a paso cómo usar ASP.NET MVC y Visual Studio para el desarrollo web.  
>   
> La tienda de música de MVC es una implementación de almacén de ejemplo ligera que vende álbumes de música en línea e implementa administración básica del sitio, en el inicio de sesión de usuario y la funcionalidad del carro de la compra.  
>   
> Esta serie de tutoriales detalla todos los pasos realizados para compilar la aplicación de ejemplo de la tienda de música de MVC de ASP.NET. Parte 8 cubre el carro de la compra con las actualizaciones de Ajax.


Se permitirá a los usuarios que coloquen álbumes en su carro sin registrar, pero tendrá que registrar como invitados consultar completa. El proceso de compra y desprotección se dividirá en dos controladores: un controlador de ShoppingCart que permite de forma anónima agregar elementos a una cesta de y un controlador de desprotección que controla el proceso de pago. Podrá iniciar con Shopping Cart en esta sección, después, se crea un proceso de retirada en la sección siguiente.

## <a name="adding-the-cart-order-and-orderdetail-model-classes"></a>Agregar las clases del modelo carro, Order y OrderDetail

Nuestros procesos Shopping Cart y desprotección hará que el uso de algunas nuevas clases. Haga clic en la carpeta de modelos y agregue una clase de carro (Cart.cs) con el código siguiente.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample1.cs)]

Esta clase es muy similar a otras personas que hemos usado hasta ahora, excepto el atributo [Key] para la propiedad de identificador de registro. Nuestros artículos del carro tendrá un identificador de cadena denominado CartID para permitir la compra anónima, pero la tabla incluye una clave principal de entero denominada RecordId. Por convención, Entity Framework Code-First espera que la clave principal para una tabla denominada carro será CartId o Id., pero, podemos fácilmente reemplazamos a través de las anotaciones o código si lo deseamos. Este es un ejemplo de cómo podemos utilizar las convenciones simples en Entity Framework Code-First cuando ajuste a nosotros, pero no nos estamos limitados por ellos cuando no lo hacen.

A continuación, agregue una clase Order (Order.cs) con el código siguiente.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample2.cs)]

Esta clase realiza un seguimiento de información de resumen y la entrega de un pedido. **No se compilará aún**porque tiene una propiedad de navegación de OrderDetails que depende de una clase que no hemos creado aún. Vamos a corregir que ahora mediante la adición de una clase denominada OrderDetail.cs, agregue el código siguiente.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample3.cs)]

Nos aseguraremos de hacer una actualización de última a nuestra clase MusicStoreEntities para incluir DbSets que exponen las nuevas clases de modelo, también incluye un DbSet&lt;intérprete&gt;. La clase MusicStoreEntities actualizada aparece como a continuación.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample4.cs)]

## <a name="managing-the-shopping-cart-business-logic"></a>Administrar la lógica de negocios de carro de la compra

A continuación, vamos a crear la clase ShoppingCart en la carpeta de modelos. El modelo ShoppingCart controla el acceso a los datos a la tabla de carro. Además, controlará la lógica de negocios a para agregar y quitar elementos del carro de la compra.

Puesto que no desea obligar a los usuarios iniciar sesión en una cuenta agregar elementos al carro de la compra, se le asignarán los usuarios un identificador temporal (mediante un GUID o identificador único global) al tener acceso al carro de la compra. Almacenaremos este identificador mediante la clase de sesión de ASP.NET.

*Nota: La sesión de ASP.NET es un lugar conveniente para almacenar información específica del usuario que expirará después de que atraviesan el sitio. Mientras un uso incorrecto del estado de sesión puede tener implicaciones de rendimiento en sitios de mayor tamaño, nuestro uso claro funcionarán bien con fines de demostración.*

La clase ShoppingCart expone los métodos siguientes:

**AddToCart** toma un álbum como parámetro y lo agrega al carro de la del usuario. Puesto que la tabla de carro realiza un seguimiento de cantidad para cada álbum, incluye una lógica para crear una nueva fila si es necesario o simplemente aumentará la cantidad si el usuario ya ha solicitado una copia del álbum.

**RemoveFromCart** toma un identificador de álbum y lo quita del carro de la del usuario. Si el usuario sólo tuviera una copia del álbum en su carro, se quita la fila.

**EmptyCart** quita todos los elementos del carro de la compra de un usuario.

**GetCartItems** recupera una lista de CartItems para mostrar o procesamiento.

**GetCount** recupera un número total de álbumes de un usuario tiene en su carro de la compra.

**GetTotal** calcula el costo total de todos los elementos en el carro.

**CreateOrder** convierte el carro de la compra en un pedido durante la fase de desprotección.

**GetCart** es un método estático que permite a nuestros controladores obtener un objeto de carro. Usa el **GetCartId** método para controlar leía el CartId la sesión del usuario. El método GetCartId requiere el objeto HttpContextBase para que pueda leer CartId del usuario de la sesión del usuario.

Aquí es el conjunto completo **clase ShoppingCart**:

[!code-csharp[Main](mvc-music-store-part-8/samples/sample5.cs)]

## <a name="viewmodels"></a>ViewModels

Nuestro controlador de carro de la compra deberá comunicarse cierta información a sus vistas compleja que no se asigna correctamente a los objetos de modelo. No queremos modificar nuestros modelos para adaptarlo a nuestro vistas; Clases de modelo deberían representar el dominio, no en la interfaz de usuario. Una solución sería pasar la información a nuestro vistas mediante la clase de elemento ViewBag, tal y como hicimos con la información de la lista desplegable de Store Manager, pero pasando una gran cantidad de información a través de ViewBag obtiene difícil de administrar.

Una solución consiste en utilizar el *ViewModel* patrón. Si utiliza este modelo, crear clases fuertemente tipadas que están optimizados para cuatro escenarios de vista específicos, y que exponen propiedades de los valores o contenido dinámico necesita nuestras plantillas de vista. Las clases de controlador, a continuación, pueden rellenar y pasar estas clases con optimización para ver a nuestra plantilla de vista que se usará. Esto permite la seguridad de tipos, comprobación en tiempo de compilación y editor IntelliSense dentro de las plantillas de vista.

Vamos a crear dos modelos de vista para su uso en el controlador de carro de la compra: el ShoppingCartViewModel contendrá el contenido del carro de la compra del usuario y la ShoppingCartRemoveViewModel se usará para mostrar información de confirmación cuando un usuario quita algo en su carro.

Vamos a crear una nueva carpeta ViewModels en la raíz de nuestro proyecto de mantener las cosas organizadas. Haga clic en el proyecto, seleccione Agregar / nueva carpeta.

![](mvc-music-store-part-8/_static/image1.jpg)

Nombre de la carpeta ViewModels.

![](mvc-music-store-part-8/_static/image1.png)

A continuación, agregue la clase ShoppingCartViewModel en la carpeta de ViewModels. Tiene dos propiedades: una lista de artículos del carro y un valor decimal para contener el precio total para todos los elementos en el carro.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample6.cs)]

Ahora agregue el ShoppingCartRemoveViewModel a la carpeta ViewModels, con las siguientes cuatro propiedades.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample7.cs)]

## <a name="the-shopping-cart-controller"></a>El controlador de carro de la compra

El controlador de carro de la compra tiene tres objetivos principales: agregar elementos a una cesta de, quitar elementos de la cesta y ven los elementos en el carro de la. Hará que el uso de las tres clases se acaba de crear: ShoppingCartViewModel, ShoppingCartRemoveViewModel y ShoppingCart. Como en los StoreController y StoreManagerController, vamos a agregar un campo que contenga una instancia de MusicStoreEntities.

Agrega un nuevo controlador de carro de la compra para el proyecto mediante la plantilla de controlador en blanco.

![](mvc-music-store-part-8/_static/image2.png)

Aquí es el controlador de ShoppingCart completa. Las acciones de índice y Agregar controlador deben ser muy familiares. Las acciones de controlador Remove y CartSummary tratar dos casos especiales, que se abordará más adelante en la sección siguiente.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample8.cs)]

## <a name="ajax-updates-with-jquery"></a>Actualizaciones de AJAX con jQuery

A continuación, vamos a crear una página de índice de carro de la compra que está fuertemente tipada en el ShoppingCartViewModel y usa la plantilla de vista de lista con el mismo método que antes.

![](mvc-music-store-part-8/_static/image3.png)

Sin embargo, en lugar de usar un Html.ActionLink para quitar elementos del carro, usaremos jQuery "conectar" el evento click para todos los vínculos en esta vista que tiene la clase HTML RemoveLink. En lugar de enviar el formulario, este controlador de eventos click solo realizará una devolución de llamada de AJAX en nuestro RemoveFromCart de la acción de controlador. El RemoveFromCart devuelve un resultado JSON serializado, que la devolución de llamada de jQuery, a continuación, analiza y realiza cuatro actualizaciones rápidas a la página mediante jQuery:

- 1. Quita el álbum eliminado de la lista
- 2. Actualiza el recuento de carro en el encabezado
- 3. Muestra un mensaje de actualización al usuario
- 4. Actualiza el precio total de carro

Puesto que se está controlando el escenario de quitar una devolución de llamada de Ajax en la vista de índice, no necesitamos una vista adicional para la acción de RemoveFromCart. Este es el código completo para la vista de /ShoppingCart/Index:

[!code-cshtml[Main](mvc-music-store-part-8/samples/sample9.cshtml)]

Para probarlo, necesitamos poder agregar elementos a nuestro carro de la compra. Actualizaremos nuestra **detalles del almacén de** vista incluya un botón "Agregar al carro". Mientras se encuentra en él, puede incluir parte de la información adicional de álbum que hemos agregado desde que se actualizó por última vez esta vista: género, intérprete, precio y carátulas de álbum. El código de la vista de detalles del almacén actualizado aparece tal y como se muestra a continuación.

[!code-cshtml[Main](mvc-music-store-part-8/samples/sample10.cshtml)]

Ahora podemos haga clic en a través de la tienda y probar agregando y quitando álbumes a y desde nuestro carro de la compra. Ejecute la aplicación y busque el índice de almacén.

![](mvc-music-store-part-8/_static/image4.png)

A continuación, haga clic en un género para ver una lista de álbumes.

![](mvc-music-store-part-8/_static/image5.png)

Al hacer clic en un título de álbum ahora muestra nuestra vista Detalles del álbum actualizada, incluido el botón "Agregar al carro".

![](mvc-music-store-part-8/_static/image6.png)

Al hacer clic en el botón "Agregar al carro" muestra la vista de índice de carro de la compra con la lista de resumen de carro de la compra.

![](mvc-music-store-part-8/_static/image7.png)

Tras la carga de su carro de la compra, puede hacer clic en la quitar del vínculo del carro para ver la actualización de Ajax al carro de la compra.

![](mvc-music-store-part-8/_static/image8.png)

Hemos desarrollado una carro que permite a los usuarios no registrados agregar elementos a su carro de la compra y en funcionamiento. En la sección siguiente, permitimos que puedan registrar y realizar el proceso de pago.


> [!div class="step-by-step"]
> [Anterior](mvc-music-store-part-7.md)
> [Siguiente](mvc-music-store-part-9.md)
