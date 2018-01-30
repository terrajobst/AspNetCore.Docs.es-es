---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
title: "Parte 9: Registro y la desprotección | Documentos de Microsoft"
author: jongalloway
description: "Esta serie de tutoriales detalla todos los pasos realizados para compilar la aplicación de ejemplo de la tienda de música de MVC de ASP.NET. Parte 9 cubre el registro y la desprotección."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: d65c5c2b-a039-463f-ad29-25cf9fb7a1ba
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
msc.type: authoredcontent
ms.openlocfilehash: 71f87043be064d24bdfb203380fb6cf651527e30
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2018
---
<a name="part-9-registration-and-checkout"></a>Parte 9: Registro y la desprotección
====================
por [Jon Galloway](https://github.com/jongalloway)

> La tienda de música de MVC es una aplicación de tutorial que presenta y explica paso a paso cómo usar ASP.NET MVC y Visual Studio para el desarrollo web.  
>   
> La tienda de música de MVC es una implementación de almacén de ejemplo ligera que vende álbumes de música en línea e implementa administración básica del sitio, en el inicio de sesión de usuario y la funcionalidad del carro de la compra.  
>   
> Esta serie de tutoriales detalla todos los pasos realizados para compilar la aplicación de ejemplo de la tienda de música de MVC de ASP.NET. Parte 9 cubre el registro y la desprotección.


En esta sección, crearemos un CheckoutController que recopilará información de pago y dirección de los compradores. Se solicitará a los usuarios a registrarse con nuestro sitio antes de desproteger, de modo que este controlador requerirá autorización.

Los usuarios se le remitirá al proceso de pago de su carro de la compra haciendo clic en el botón "Desproteger".

![](mvc-music-store-part-9/_static/image1.jpg)

Si el usuario no ha iniciado sesión, le pedirá que.

![](mvc-music-store-part-9/_static/image1.png)

Cuando inician sesión correctamente, el usuario, a continuación, se muestra la vista de direcciones y pagos.

![](mvc-music-store-part-9/_static/image2.png)

Una vez que ha rellenado el formulario y de enviar el pedido, mostrará la pantalla de confirmación de pedido.

![](mvc-music-store-part-9/_static/image3.png)

Intentar ver un orden no existente o en un orden que no pertenece a la se mostrará el Error, consulte.

![](mvc-music-store-part-9/_static/image4.png)

## <a name="migrating-the-shopping-cart"></a>Migrar el carro de la compra

Mientras el proceso de compra es anónimo, cuando el usuario hace clic en el botón de extracción del repositorio, le pedirá que registre e inicie sesión. Los usuarios esperarán a que se mantendrá su información del carro de compra entre las visitas, por lo que necesitamos asociar la información del carro de compra a un usuario cuando complete el registro o inicio de sesión.

Esto es realmente muy sencilla hacer, según nuestra clase ShoppingCart ya tiene un método que se asociará a todos los elementos en el carro actual con un nombre de usuario. Solo se deberá llamar a este método cuando un usuario completa el registro o inicio de sesión.

Abra la **AccountController** clase que agregamos al que estábamos configurar la pertenencia y la autorización. Agregar un utilizando la instrucción que hacen referencia a MvcMusicStore.Models, a continuación, agregue el siguiente método MigrateShoppingCart:

[!code-csharp[Main](mvc-music-store-part-9/samples/sample1.cs)]

A continuación, modifique la acción de envío de inicio de sesión para llamar a MigrateShoppingCart después de que se ha validado el usuario, tal y como se muestra a continuación:

[!code-csharp[Main](mvc-music-store-part-9/samples/sample2.cs)]

Realizar el mismo cambio en el registro exponga acción, inmediatamente después de que se haya creado correctamente la cuenta de usuario:

[!code-csharp[Main](mvc-music-store-part-9/samples/sample3.cs)]

¡Eso es todo - ahora un carro de la compra anónimo se transferirán automáticamente a una cuenta de usuario en un registro correcto o inicio de sesión.

## <a name="creating-the-checkoutcontroller"></a>Crear el CheckoutController

Haga doble clic en la carpeta Controllers y agregue un nuevo controlador al proyecto denominado CheckoutController mediante la plantilla de controlador en blanco.

![](mvc-music-store-part-9/_static/image5.png)

En primer lugar, agregue el atributo Authorize encima de la declaración de clase de controlador para obligar a los usuarios registrar antes de extracción del repositorio:

[!code-csharp[Main](mvc-music-store-part-9/samples/sample4.cs)]

*Nota: Esto es similar al cambio que se haya realizado previamente para el StoreManagerController, pero en ese caso, el atributo Authorize requiere que el usuario sea en un rol de administrador. En el controlador de desprotección, estamos requerir al usuario se registran en pero no se requiera que ser administradores.*

Por simplicidad, no se trabaja con la información de pago en este tutorial. En su lugar, se permite que los usuarios extraigan del repositorio utilizando un código promocional. Se almacenará este código promocional utilizando una constante denominada PromoCode.

Como se muestra en el StoreController, declararemos un campo que contenga una instancia de la clase MusicStoreEntities, denominada storeDB. Para hacer uso de la clase MusicStoreEntities, tendrá que agregar un elemento using instrucción del espacio de nombres MvcMusicStore.Models. La parte superior de nuestro controlador desprotección aparece debajo.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample5.cs)]

El CheckoutController tendrá las siguientes acciones de controlador:

**AddressAndPayment (método GET)** mostrará un formulario para permitir al usuario que escriba su información.

**AddressAndPayment (método POST)** se valida la entrada y procesar el pedido.

**Completa** se mostrará cuando un usuario haya finalizado correctamente el proceso de pago. Esta vista incluirá el número de pedido del usuario, como confirmación.

En primer lugar, vamos a cambiar el nombre de la acción de controlador de índice (que se generó cuando se creó el controlador) a AddressAndPayment. Esta acción de controlador solo muestra el formulario de comprobación, por lo que no requiere ninguna información sobre el modelo.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample6.cs)]

El método POST AddressAndPayment seguirá el mismo patrón que hemos usado en el StoreManagerController: se intentará acepte el envío del formulario y completar el pedido y volver a mostrará el formulario si se produce un error.

Después de validar la entrada de formulario cumple los requisitos de validación de un pedido, se comprobará el valor de formulario PromoCode directamente. Suponiendo que todo es correcto, que se guardará la información actualizada con el orden, indicar al objeto de ShoppingCart para completar el proceso de pedido y redirigir al finalizar la acción.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample7.cs)]

Tras la finalización correcta del proceso de extracción del repositorio, los usuarios se redirigirán a la acción del controlador completa. Esta acción se realizará una comprobación simple para validar que el orden realmente pertenecen al usuario que ha iniciado la sesión antes de mostrar el número de pedido como una confirmación.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample8.cs)]

*Nota: La vista de Error se creó automáticamente para que podamos en la carpeta /Views/Shared cuando se inició el proyecto.*

El código completo de CheckoutController es como sigue:

[!code-csharp[Main](mvc-music-store-part-9/samples/sample9.cs)]

## <a name="adding-the-addressandpayment-view"></a>Agregar la vista de AddressAndPayment

Ahora, vamos a crear la vista de AddressAndPayment. Haga doble clic en uno de las acciones del controlador de AddressAndPayment y agregar una vista denominada AddressAndPayment que está fuertemente tipado como un pedido y usa la plantilla de edición, tal y como se muestra a continuación.

![](mvc-music-store-part-9/_static/image6.png)

Esta vista hará que el uso de dos de las técnicas que hemos observado durante la creación de la vista de StoreManagerEdit:

- Usaremos Html.EditorForModel() para mostrar los campos de formulario para el modelo de orden
- Aprovechamos mediante una clase Order con atributos de validación de reglas de validación

Comenzaremos por actualizar el código del formulario para que use Html.EditorForModel(), seguido de un cuadro de texto adicional para el código de promoción. El código completo de la vista de AddressAndPayment se muestra a continuación.

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample10.cshtml)]

## <a name="defining-validation-rules-for-the-order"></a>Definir reglas de validación para el pedido

Ahora que se ha configurado la vista, se configurará las reglas de validación para nuestro modelo de orden tal y como se hacía anteriormente para el modelo de álbum. Haga doble clic en la carpeta Models y agregue una clase denominada pedido. Además de los atributos de validación que hemos usado previamente para el álbum, también usaremos una expresión Regular para validar la dirección de correo electrónico del usuario.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample11.cs)]

Al intentar enviar el formulario con la que falta o información no válida ahora mostrará el mensaje de error mediante la validación del lado cliente.

![](mvc-music-store-part-9/_static/image7.png)

Bien, que hemos hecho la mayor parte de la tarea ardua de dicho proceso; sólo tenemos algunas posibilidades and extremos para finalizar. Tenemos que agregar dos vistas simples y necesitamos a cargo de la entrega de la información del carro durante el proceso de inicio de sesión.

## <a name="adding-the-checkout-complete-view"></a>Agregar la vista completa de desprotección

La vista completa de desprotección es bastante sencilla, como solo necesita para mostrar el identificador de pedido. Haga doble clic en la acción de controlador completa y agregar una vista con el nombre completo que está fuertemente tipado como entero.

![](mvc-music-store-part-9/_static/image8.png)

Ahora se actualizará el código de la vista para mostrar el identificador de pedido, tal y como se muestra a continuación.

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample12.cshtml)]

## <a name="updating-the-error-view"></a>Error de la actualización, consulte

La plantilla predeterminada incluye una vista de Error en la carpeta de vistas compartido para que pueda volver a usar en otro lugar en el sitio. Esta vista de Error contiene un error muy simple y no usa nuestro sitio de diseño, por lo que deberá actualizarla.

Puesto que se trata de una página de error genérico, el contenido es muy sencillo. Incluiremos un mensaje y un vínculo para navegar a la página anterior en el historial si el usuario desea volver a intentar su acción.

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample13.cshtml)]


>[!div class="step-by-step"]
[Anterior](mvc-music-store-part-8.md)
[Siguiente](mvc-music-store-part-10.md)
