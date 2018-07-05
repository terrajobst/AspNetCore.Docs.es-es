---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
title: 'Parte 9: Registro y la desprotección | Microsoft Docs'
author: jongalloway
description: Esta serie de tutoriales detalla todos los pasos realizados para compilar la aplicación de ejemplo de Music Store de ASP.NET MVC. Parte 9 cubre el registro y la desprotección.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: d65c5c2b-a039-463f-ad29-25cf9fb7a1ba
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
msc.type: authoredcontent
ms.openlocfilehash: 935729be0dce790c6fce2e2e982ee063318d64dd
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37367015"
---
<a name="part-9-registration-and-checkout"></a>Parte 9: Registro y desprotección
====================
por [Jon Galloway](https://github.com/jongalloway)

> El Store de música de MVC es una aplicación de tutorial que presenta y explica paso a paso cómo usar ASP.NET MVC y Visual Studio para el desarrollo web.  
>   
> El Store de música de MVC es una implementación de almacén de ejemplo ligera que vende álbumes de música en línea e implementa la administración básica del sitio, inicio de sesión de usuario y funcionalidad del carro de la compra.  
>   
> Esta serie de tutoriales detalla todos los pasos realizados para compilar la aplicación de ejemplo de Music Store de ASP.NET MVC. Parte 9 cubre el registro y la desprotección.


En esta sección, crearemos un CheckoutController que recopile información de pago y dirección del comprador. Se requerirá que los usuarios se registren con nuestro sitio antes de desproteger, por lo que este controlador requerirá autorización.

Los usuarios navegará al proceso de pago de su carro de la compra haciendo clic en el botón "Desproteger".

![](mvc-music-store-part-9/_static/image1.jpg)

Si el usuario no ha iniciado sesión, se pedirá a.

![](mvc-music-store-part-9/_static/image1.png)

Cuando inician sesión correctamente, el usuario, a continuación, se muestra la vista de direcciones y pagos.

![](mvc-music-store-part-9/_static/image2.png)

Una vez que hayan rellenado el formulario y de enviar el pedido, se mostrará la pantalla de confirmación de pedido.

![](mvc-music-store-part-9/_static/image3.png)

Al intentar ver un orden que no existe o un pedido que no pertenece a la se mostrará la vista de Error.

![](mvc-music-store-part-9/_static/image4.png)

## <a name="migrating-the-shopping-cart"></a>Migrar el carro de la compra

Mientras el proceso de compra es anónimo, cuando el usuario hace clic en el botón Checkout, le pedirá que registre e inicie sesión. Los usuarios esperan que se mantendrá su información del carro de la compra entre las visitas, por lo que necesitamos asociar la información del carro de la compra a un usuario cuando se completa el registro o inicio de sesión.

Esto es realmente muy sencillo hacer, como nuestra clase ShoppingCart ya tiene un método que se asociará a todos los elementos en el carro actual con un nombre de usuario. Sólo necesitamos llamar a este método cuando un usuario completa el registro o inicio de sesión.

Abra el **AccountController** clase que agregamos al que estábamos Configurar pertenencia y autorización. Agregar un utilizando la instrucción que haga referencia MvcMusicStore.Models, a continuación, agregue el siguiente método MigrateShoppingCart:

[!code-csharp[Main](mvc-music-store-part-9/samples/sample1.cs)]

A continuación, modifique la acción posterior a la de inicio de sesión para llamar a MigrateShoppingCart después de que se haya validado el usuario, como se muestra a continuación:

[!code-csharp[Main](mvc-music-store-part-9/samples/sample2.cs)]

Realizar el mismo cambio en el registro de registrar la acción, inmediatamente después de que se haya creado correctamente la cuenta de usuario:

[!code-csharp[Main](mvc-music-store-part-9/samples/sample3.cs)]

Eso es todo: ahora un carro de compra anónimo se transferirán automáticamente a una cuenta de usuario tras el inicio de sesión o registro correcto.

## <a name="creating-the-checkoutcontroller"></a>Crear el CheckoutController

Haga doble clic en la carpeta Controllers y agregue un nuevo controlador al proyecto denominado CheckoutController mediante la plantilla de controlador en blanco.

![](mvc-music-store-part-9/_static/image5.png)

En primer lugar, agregue el atributo Authorize encima de la declaración de clase de controlador para requerir a los usuarios registrar antes de la desprotección:

[!code-csharp[Main](mvc-music-store-part-9/samples/sample4.cs)]

*Nota: Esto es similar al cambio que hemos realizado anteriormente en el StoreManagerController, pero en ese caso, el atributo Authorize requiere que el usuario sea en un rol de administrador. En el controlador de desprotección, estamos requerir al usuario iniciar sesión pero no se requiera que sean administradores.*

Por simplicidad, no se trabaja con información de pago en este tutorial. En su lugar, se permite que los usuarios desproteger un código promocional. Almacenamos este código promocional con una constante denominada PromoCode.

Como se muestra en el StoreController, declararemos un campo que contenga una instancia de la clase MusicStoreEntities, denominada storeDB. Para hacer que el uso de la clase MusicStoreEntities, necesitamos agregar un mediante declaración para el espacio de nombres MvcMusicStore.Models. La parte superior de nuestro controlador desprotección aparece debajo.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample5.cs)]

El CheckoutController tendrá las siguientes acciones de controlador:

**AddressAndPayment (método GET)** mostrará un formulario para permitir al usuario que escriba su información.

**AddressAndPayment (método POST)** validará la entrada y procesar el pedido.

**Completa** se mostrará después de que un usuario ha finalizado correctamente el proceso de pago. Esta vista incluirá el número de pedido del usuario, como confirmación.

En primer lugar, vamos a cambiar el nombre de la acción Index del controlador (que se generó cuando se creó el controlador) para AddressAndPayment. Esta acción de controlador solo muestra el formulario de comprobación, por lo que no requiere ninguna información de modelo.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample6.cs)]

El método POST AddressAndPayment seguirá el mismo patrón que hemos usado en el StoreManagerController: se intentará acepte el envío del formulario y completar el pedido y volver a mostrará el formulario si se produce un error.

Después de validar la entrada de formulario cumple con nuestros requisitos de validación para un pedido, se comprobará el valor de formulario PromoCode directamente. Suponiendo que todo es correcto, que se guardará la información actualizada con el orden, indicarle al objeto ShoppingCart completar el proceso de pedido y redirigir a completar la acción.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample7.cs)]

Tras la finalización correcta del proceso de retirada, los usuarios se redirigirán a la acción del controlador completo. Esta acción llevará a cabo una comprobación simple para validar que el orden realmente pertenecen al usuario que ha iniciado sesión antes de mostrar el número de pedido como una confirmación.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample8.cs)]

*Nota: La vista de Error se creó automáticamente para nosotros en la carpeta /Views/Shared cuando empezamos con el proyecto.*

El código completo de CheckoutController es como sigue:

[!code-csharp[Main](mvc-music-store-part-9/samples/sample9.cs)]

## <a name="adding-the-addressandpayment-view"></a>Agregar la vista de AddressAndPayment

Ahora, vamos a crear la vista AddressAndPayment. Haga doble clic en una de las acciones de controlador AddressAndPayment y agregar una vista denominada AddressAndPayment que está fuertemente tipada como un pedido y utiliza la plantilla de edición, como se muestra a continuación.

![](mvc-music-store-part-9/_static/image6.png)

Esta vista hará que el uso de dos de las técnicas que hemos examinados durante la compilación de la vista StoreManagerEdit:

- Usaremos Html.EditorForModel() para mostrar los campos de formulario para el modelo de orden
- Aprovechamos las reglas de validación mediante una clase Order con atributos de validación

Comenzaremos por actualizar el código del formulario para que use Html.EditorForModel(), seguido de un cuadro de texto adicional para el código de promoción. El código completo de la vista AddressAndPayment se muestra a continuación.

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample10.cshtml)]

## <a name="defining-validation-rules-for-the-order"></a>Definir reglas de validación para el pedido

Ahora que nuestro punto de vista se ha configurado, se configurará las reglas de validación para nuestro modelo de pedido como hicimos anteriormente para el modelo de álbum. Haga doble clic en la carpeta Models y agregue una clase denominada pedido. Además de los atributos de validación que se ha usado previamente para el álbum, también usaremos una expresión Regular para validar la dirección de correo electrónico del usuario.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample11.cs)]

Intenta enviar el formulario que falta o información no válida ahora mostrará el mensaje de error con la validación del lado cliente.

![](mvc-music-store-part-9/_static/image7.png)

Bien, lo que hicimos la mayoría del trabajo duro para el proceso de retirada; sólo tenemos unos misceláneos para finalizar. Necesitamos agregar dos vistas simples y necesitamos que se encargue de la entrega de la información del carro de compra durante el proceso de inicio de sesión.

## <a name="adding-the-checkout-complete-view"></a>Agregar la vista completa de desprotección

La vista completa de desprotección es bastante sencilla, ya que solo se necesita para mostrar el identificador de pedido. Haga doble clic en la acción de controlador completa y agregar una vista denominada Complete que está fuertemente tipada como entero.

![](mvc-music-store-part-9/_static/image8.png)

Ahora actualizaremos el código de vista para mostrar el identificador de pedido, tal como se muestra a continuación.

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample12.cshtml)]

## <a name="updating-the-error-view"></a>Actualizando la vista de Error

La plantilla predeterminada incluye una vista de Error en la carpeta de vistas compartidas, por lo que se pueda volver a usar en otro lugar en el sitio. Esta vista de Error contiene un error muy simple y no utiliza nuestro sitio de diseño, por lo que se actualizará.

Puesto que se trata de una página de error genérico, el contenido es muy sencillo. Incluiremos un mensaje y un vínculo para ir a la página anterior del historial si el usuario desea volver a intentar su acción.

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample13.cshtml)]


> [!div class="step-by-step"]
> [Anterior](mvc-music-store-part-8.md)
> [Siguiente](mvc-music-store-part-10.md)
