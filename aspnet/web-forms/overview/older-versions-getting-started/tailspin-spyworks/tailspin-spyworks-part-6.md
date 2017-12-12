---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
title: 'Parte 6: Pertenencia a ASP.NET | Documentos de Microsoft'
author: JoeStagner
description: "Esta serie de tutoriales detalla todos los pasos realizados para compilar la aplicación de ejemplo de Tailspin Spyworks. Parte 6 agrega la pertenencia a ASP.NET."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: f70a310c-9557-4743-82cb-655265676d39
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
msc.type: authoredcontent
ms.openlocfilehash: efb0e2bed1172f42c7f1539f016fba305c47e3eb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="part-6-aspnet-membership"></a>Parte 6: Pertenencia a ASP.NET
====================
por [Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks muestra cómo extraordinariamente simple es crear aplicaciones eficaces y escalables para la plataforma. NET. Muestra cómo usar las características nuevas en ASP.NET 4 para crear una tienda en línea, incluidos los de la compra, la desprotección y la administración.
> 
> Esta serie de tutoriales detalla todos los pasos realizados para compilar la aplicación de ejemplo de Tailspin Spyworks. Parte 6 agrega la pertenencia a ASP.NET.


## <a id="_Toc260221672"></a>Trabajar con la pertenencia a ASP.NET

![](tailspin-spyworks-part-6/_static/image1.png)

Haga clic en seguridad

![](tailspin-spyworks-part-6/_static/image1.jpg)

Asegúrese de que estamos usando la autenticación de formularios.

![](tailspin-spyworks-part-6/_static/image2.jpg)

Utilice el vínculo "Create User" para crear un par de usuarios.

![](tailspin-spyworks-part-6/_static/image3.jpg)

Cuando haya finalizado, consulte la ventana Explorador de soluciones y actualice la vista.

![](tailspin-spyworks-part-6/_static/image2.png)

Tenga en cuenta que la ASPNETDB. Se ha creado un problema de MDF. Este archivo contiene las tablas para admitir los servicios principales de ASP.NET como pertenencia.

Ahora podemos comenzar por implementar el proceso de extracción del repositorio.

Empiece por crear una página caja.aspx.

La página caja.aspx solo debe estar disponible para los usuarios que han iniciado sesión, por lo que se restringirá el acceso a registrados en los usuarios y redirige a los usuarios que no está conectados a la página de inicio de sesión.

Para ello, vamos a agregar lo siguiente a la sección de configuración de nuestro archivo web.config.

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample1.xml)]

La plantilla para las aplicaciones de formularios Web Forms de ASP.NET se ha agregado una sección de autenticación a nuestro archivo web.config y establece la página de inicio de sesión predeterminada automáticamente.

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample2.xml)]

Debemos modificamos Login.aspx archivo de código subyacente para migrar un carro de la compra anónimo cuando el usuario inicia sesión. Cambiar la página\_como se indica a continuación, el evento de carga.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample3.cs)]

A continuación, agregue un controlador de eventos "LoggedIn" similar al siguiente para establecer el nombre de sesión para el usuario recién ha iniciado sesión y cambie el identificador de sesión temporal en el carro de la compra a la el usuario llamando al método MigrateCart en nuestra clase MyShoppingCart. (Se implementa en el archivo. cs)

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample4.cs)]

Implemente el método MigrateCart() similar a éste.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample5.cs)]

En caja.aspx usaremos un EntityDataSource y un control GridView en nuestra página retirar tanto como hicimos en nuestra página de carro de la compra.

[!code-aspx[Main](tailspin-spyworks-part-6/samples/sample6.aspx)]

Tenga en cuenta que nuestro control GridView especifica un controlador de eventos "ondatabound" denominado MyList\_RowDataBound así que vamos a implementar ese controlador de eventos similar al siguiente.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample7.cs)]

Esta manera se mantendrá método total acumulativo de la compra carro como cada fila está enlazada y actualiza la fila de la parte inferior del control GridView.

Hemos implementado una presentación "revisión" de la orden a colocarse en esta fase.

Vamos a controlar un escenario de carro vacío agregando unas pocas líneas de código a nuestra página de\_eventos de carga:

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample8.cs)]

Cuando el usuario hace clic en el botón "Enviar", se ejecutará el código siguiente en el controlador de eventos de haga clic de botón de envío.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample9.cs)]

La "sustancia" del proceso de envío de pedido es para que se implementen en el método SubmitOrder() de nuestra clase MyShoppingCart.

SubmitOrder hará lo siguiente:

- Tomar todos los elementos de línea en el carro de la compra y utilizarlas para crear un nuevo registro de pedido y los registros de OrderDetails asociados.
- Calcular la fecha de envío.
- Desactive el carro de la compra.


[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample10.cs)]

Para los propósitos de esta aplicación de ejemplo se podrá calcular una fecha de envío con solo agregar dos días a la fecha actual.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample11.cs)]

Ejecutar la aplicación ahora nos permitirá para probar el proceso de compra de principio a fin.

>[!div class="step-by-step"]
[Anterior](tailspin-spyworks-part-5.md)
[Siguiente](tailspin-spyworks-part-7.md)
