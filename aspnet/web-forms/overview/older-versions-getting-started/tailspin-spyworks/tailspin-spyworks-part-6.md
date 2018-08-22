---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
title: 'Parte 6: Pertenencia ASP.NET | Microsoft Docs'
author: JoeStagner
description: Esta serie de tutoriales detalla todos los pasos que se tarda en compilar la aplicación de ejemplo Tailspin Spyworks. Parte 6 agrega la pertenencia a ASP.NET.
ms.author: riande
ms.date: 07/21/2010
ms.assetid: f70a310c-9557-4743-82cb-655265676d39
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
msc.type: authoredcontent
ms.openlocfilehash: c847db058bc03115210f12eeb0c3c76fecc8a31e
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835885"
---
<a name="part-6-aspnet-membership"></a>Parte 6: Pertenencia ASP.NET
====================
por [Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks demuestra cómo extraordinariamente simple es crear aplicaciones eficaces y escalables para la plataforma. NET. Resalta cómo usar las características nuevas en ASP.NET 4 para crear una tienda en línea, incluida la compra, la desprotección y la administración.
> 
> Esta serie de tutoriales detalla todos los pasos que se tarda en compilar la aplicación de ejemplo Tailspin Spyworks. Parte 6 agrega la pertenencia a ASP.NET.


## <a id="_Toc260221672"></a>  Trabajar con la pertenencia a ASP.NET

![](tailspin-spyworks-part-6/_static/image1.png)

Haga clic en seguridad

![](tailspin-spyworks-part-6/_static/image1.jpg)

Asegúrese de que estamos usando autenticación de formularios.

![](tailspin-spyworks-part-6/_static/image2.jpg)

Use el vínculo "Crear usuario" para crear un par de usuarios.

![](tailspin-spyworks-part-6/_static/image3.jpg)

Cuando haya terminado, consulte la ventana Explorador de soluciones y actualice la vista.

![](tailspin-spyworks-part-6/_static/image2.png)

Tenga en cuenta que el ASPNETDB. MDF bien se ha creado. Este archivo contiene las tablas para admitir los servicios de ASP.NET core, como la pertenencia.

Ahora podemos comenzar por implementar el proceso de pago.

Empiece por crear una página caja.aspx.

La página caja.aspx solo debe estar disponible para los usuarios que han iniciado sesión, por lo que nos restringirá el acceso que ha iniciado sesión y redirigir los usuarios que no ha iniciado sesión en la página de inicio de sesión.

Para ello, vamos a agregar lo siguiente a la sección de configuración del archivo web.config.

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample1.xml)]

La plantilla para las aplicaciones de formularios Web Forms de ASP.NET se ha agregado una sección de autenticación a nuestro archivo web.config y es necesario establecer la página de inicio de sesión predeterminada automáticamente.

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample2.xml)]

Debemos modificamos Login.aspx archivo de código subyacente para migrar un carro de compra anónimo cuando el usuario inicia sesión. Cambiar la página\_como se indica a continuación, el evento de carga.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample3.cs)]

A continuación, agregue un controlador de eventos "LoggedIn" similar al siguiente para establecer el nombre de sesión para el usuario recién conectado y cambie el identificador de sesión temporal en el carro de la compra a del usuario llamando al método MigrateCart en nuestra clase MyShoppingCart. (Se implementa en el archivo. cs)

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample4.cs)]

Implemente el método MigrateCart() como éste.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample5.cs)]

En caja.aspx usaremos un EntityDataSource y un control GridView en nuestra página retirar tanto como hicimos en nuestra página del carro de la compra.

[!code-aspx[Main](tailspin-spyworks-part-6/samples/sample6.aspx)]

Tenga en cuenta que nuestro control GridView especifica un controlador de eventos "ondatabound" denominado MyList\_RowDataBound así que vamos a implementar ese controlador de eventos similar al siguiente.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample7.cs)]

Este mantiene método total de la compra el carro como cada fila está enlazada y actualiza la fila de la parte inferior del control GridView.

Hemos implementado una presentación de "revisión" de la orden a colocarse en esta fase.

Vamos a controlar un escenario de carro vacío mediante la adición de unas pocas líneas de código a nuestra página\_eventos de carga:

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample8.cs)]

Cuando el usuario hace clic en el botón "Enviar", se ejecutará el código siguiente en el controlador de envío de evento Click de botón.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample9.cs)]

Es la "carne" el proceso de envío de pedido se implementa en el método SubmitOrder() de nuestra clase MyShoppingCart.

SubmitOrder hará lo siguiente:

- Tomar todos los elementos de línea en el carro de la compra y usarlos para crear un nuevo registro de pedido y los registros de OrderDetails asociados.
- Calcular la fecha de envío.
- Desactive el carro de la compra.


[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample10.cs)]

Para los fines de esta aplicación de ejemplo se a calcular una fecha de envío, agregando dos días hasta la fecha actual.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample11.cs)]

Ejecutar la aplicación ahora nos permitirá probar el proceso de compra de principio a fin.

> [!div class="step-by-step"]
> [Anterior](tailspin-spyworks-part-5.md)
> [Siguiente](tailspin-spyworks-part-7.md)
