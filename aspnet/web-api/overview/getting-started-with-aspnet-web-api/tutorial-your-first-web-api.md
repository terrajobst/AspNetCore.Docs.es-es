---
uid: web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
title: Introducción a ASP.NET Web API 2 (C#)
author: MikeWasson
description: HTTP no es solo para servir páginas web. También es una plataforma eficaz para la creación de API que exponen los datos y servicios. HTTP es sencillo, flexible y ubiq...
ms.author: riande
ms.date: 11/28/2017
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
msc.type: authoredcontent
ms.openlocfilehash: 14796ce31187a0f6a6331b46477fadc77e281c3b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835389"
---
<a name="get-started-with-aspnet-web-api-2-c"></a>Introducción a ASP.NET Web API 2 (C#)
====================
por [Mike Wasson](https://github.com/MikeWasson)

[Descargue el proyecto completado](https://code.msdn.microsoft.com/Sample-code-of-Getting-c56ccb28)

HTTP no es solo para servir páginas web. HTTP es también una plataforma eficaz para la creación de API que exponen los datos y servicios. HTTP es sencillo, flexible y ubicua. Casi cualquier plataforma que se puede considerar tiene una biblioteca HTTP, por lo que los servicios HTTP pueden llegar a una amplia gama de clientes, incluidos los exploradores, dispositivos móviles y aplicaciones de escritorio tradicionales.
 
ASP.NET Web API es un marco para crear las API web sobre .NET Framework. En este tutorial, usará la API Web de ASP.NET para crear una API web que devuelve una lista de productos.

 
 ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software que se usa en el tutorial
  
 - [Visual Studio 2017](https://www.visualstudio.com/downloads/)
 - Web API 2

Consulte [crear una API web con ASP.NET Core y Visual Studio para Windows](https://docs.microsoft.com/aspnet/core/tutorials/first-web-api) para una versión más reciente de este tutorial.

## <a name="create-a-web-api-project"></a>Crear un proyecto de API Web

En este tutorial, usará la API Web de ASP.NET para crear una API web que devuelve una lista de productos. La página web front-end utiliza jQuery para mostrar los resultados.

![](tutorial-your-first-web-api/_static/image1.png)

Inicie Visual Studio y seleccione **nuevo proyecto** desde el **iniciar** página. O bien, en el **archivo** menú, seleccione **New** y, a continuación, **proyecto**.

En el **plantillas** panel, seleccione **plantillas instaladas** y expanda el **Visual C#** nodo. En **Visual C#**, seleccione **Web**. En la lista de plantillas de proyecto, seleccione **aplicación Web ASP.NET**. Denomine el proyecto "ProductsApp" y haga clic en **Aceptar**.

![](tutorial-your-first-web-api/_static/image2.png)

En el **nuevo proyecto ASP.NET** cuadro de diálogo, seleccione el **vacía** plantilla. En &quot;agregar carpetas y referencias centrales para&quot;, comprobar **API Web**. Haga clic en **Aceptar**.

![](tutorial-your-first-web-api/_static/image3.png)

> [!NOTE]
> También puede crear un proyecto de API Web mediante la &quot;API Web&quot; plantilla. La plantilla API Web usa ASP.NET MVC para proporcionar páginas de Ayuda de API. Estoy usando la plantilla vacía para este tutorial porque quiero mostrarles API Web sin MVC. En general, no es necesario conocer ASP.NET MVC para usar Web API.


## <a name="adding-a-model"></a>Adición de un modelo

Un *modelo* es un objeto que representa los datos de la aplicación. ASP.NET Web API puede serializar automáticamente el modelo en JSON, XML o algún otro formato y, a continuación, escribe los datos serializados en el cuerpo del mensaje de respuesta HTTP. Siempre que un cliente puede leer el formato de serialización, que se puede deserializar el objeto. La mayoría de los clientes pueden analizar XML o JSON. Además, el cliente puede indicar el formato que desee estableciendo el encabezado Accept del mensaje de solicitud HTTP.

Comencemos por crear un modelo simple que representa un producto.

Si el Explorador de soluciones no está visible, haga clic en el **vista** menú y seleccione **el Explorador de soluciones**. En el Explorador de soluciones, haga clic en la carpeta Models. En el menú contextual, seleccione **agregar** , a continuación, seleccione **clase**.

![](tutorial-your-first-web-api/_static/image4.png)

Nombre de la clase &quot;producto&quot;. Agregue las siguientes propiedades para el `Product` clase.

[!code-csharp[Main](tutorial-your-first-web-api/samples/sample1.cs)]

## <a name="adding-a-controller"></a>Agregar un controlador

En la API Web, un *controlador* es un objeto que controla las solicitudes HTTP. Vamos a agregar un controlador que puede devolver una lista de productos o un único producto especificado por identificador.

> [!NOTE]
> Si ha usado ASP.NET MVC, ya están familiarizados con los controladores. Controladores Web API son similares a los controladores MVC, pero heredan la **ApiController** clase en lugar de la **controlador** clase.

En **el Explorador de soluciones**, haga clic en la carpeta Controllers. Seleccione **agregar** y, a continuación, seleccione **controlador**.

![](tutorial-your-first-web-api/_static/image5.png)

En el **agregar Scaffold** cuadro de diálogo, seleccione **controlador Web API - vacío**. Haga clic en **Agregar**.

![](tutorial-your-first-web-api/_static/image6.png)

En el **Agregar controlador** cuadro de diálogo, el nombre del controlador &quot;ProductsController&quot;. Haga clic en **Agregar**.

![](tutorial-your-first-web-api/_static/image7.png)

La técnica de scaffolding crea un archivo denominado ProductsController.cs en la carpeta Controllers.

![](tutorial-your-first-web-api/_static/image8.png)

> [!NOTE]
> No es necesario poner los controladores en una carpeta denominada controladores. El nombre de carpeta es una manera cómoda de organizar los archivos de origen.


Si este archivo ya no está abierto, haga doble clic en el archivo para abrirlo. Reemplace el código de este archivo por lo siguiente:

[!code-csharp[Main](tutorial-your-first-web-api/samples/sample2.cs)]

Para simplificar el ejemplo, los productos se almacenan en una matriz fija dentro de la clase de controlador. Por supuesto, en una aplicación real, podría consultar una base de datos o utilizar algún otro origen de datos externo.

El controlador define dos métodos que devuelven productos:

- El `GetAllProducts` método devuelve la lista completa de productos como un **IEnumerable&lt;producto&gt;**  tipo.
- El `GetProduct` método busca un solo producto por su identificador.

Ya está. Tiene una API web de trabajo. Cada método en el controlador corresponde a uno o varios de los URI:

| Método de controlador | Identificador URI |
| --- | --- |
| GetAllProducts | productos/api / |
| GetProduct | /API/productos/*Id.* |

Para el `GetProduct` método, el *id* en el URI es un marcador de posición. Por ejemplo, para obtener el producto con el Id. de 5, el URI es `api/products/5`.

Para obtener más información acerca de cómo API Web enruta las solicitudes HTTP a los métodos de controlador, consulte [Routing in ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).

## <a name="calling-the-web-api-with-javascript-and-jquery"></a>Llamar a la API Web con Javascript y jQuery

En esta sección, vamos a agregar una página HTML que se usa AJAX para llamar a la API web. Vamos a usar jQuery para realizar las llamadas AJAX y también para actualizar la página con los resultados.

En el Explorador de soluciones, haga clic en el proyecto y seleccione **agregar**, a continuación, seleccione **nuevo elemento**.

![](tutorial-your-first-web-api/_static/image9.png)

En el **Agregar nuevo elemento** cuadro de diálogo, seleccione el **Web** nodo bajo **Visual C#** y, a continuación, seleccione el **página HTML** elemento. Nombre de la página &quot;index.html&quot;.

![](tutorial-your-first-web-api/_static/image10.png)

Reemplace todo el contenido de este archivo por lo siguiente:

[!code-html[Main](tutorial-your-first-web-api/samples/sample3.html)]

Existen varias formas de obtener jQuery. En este ejemplo, usé la [Microsoft Ajax CDN](../../../ajax/cdn/overview.md). También puede descargarlo desde [ http://jquery.com/ ](http://jquery.com/)y ASP.NET "API Web" plantilla de proyecto incluye también jQuery.

### <a name="getting-a-list-of-products"></a>Obtener una lista de productos

Para obtener una lista de productos, envíe una solicitud HTTP GET a &quot;/api/productos&quot;.

JQuery [getJSON](http://api.jquery.com/jQuery.getJSON/) función envía una solicitud AJAX. Para la respuesta contiene la matriz de objetos JSON. El `done` función especifica una devolución de llamada que se llama si la solicitud se realiza correctamente. En la devolución de llamada, actualizamos el DOM con la información del producto.

[!code-html[Main](tutorial-your-first-web-api/samples/sample4.html)]

### <a name="getting-a-product-by-id"></a>Obtención de un producto por Id.

Para obtener un producto por identificador, envíe una solicitud HTTP GET a &quot;/API/productos/*id*&quot;, donde *id* es el identificador de producto.

[!code-javascript[Main](tutorial-your-first-web-api/samples/sample5.js)]

Aún utilizamos `getJSON` para enviar la solicitud de AJAX, pero esta vez se ponga el Id. del URI de solicitud. La respuesta de esta solicitud es una representación JSON de un solo producto.

## <a name="running-the-application"></a>Ejecutar la aplicación

Presione F5 para iniciar la depuración de la aplicación. La página web debe tener el siguiente aspecto:

![](tutorial-your-first-web-api/_static/image11.png)

Para obtener un producto por identificador, escriba el identificador y haga clic en Buscar:

![](tutorial-your-first-web-api/_static/image12.png)

Si escribe un identificador no válido, el servidor devuelve un error HTTP:

![](tutorial-your-first-web-api/_static/image13.png)

## <a name="using-f12-to-view-the-http-request-and-response"></a>Usar F12 para ver la solicitud y respuesta HTTP

Cuando se trabaja con un servicio HTTP, puede ser muy útil ver la solicitud HTTP y los mensajes de solicitud. Puede hacerlo mediante el uso de herramientas de desarrollo F12 de Internet Explorer 9. En Internet Explorer 9, presione **F12** para abrir las herramientas. Haga clic en el **red** pestaña y presione **Iniciar captura**. Ahora, vaya a la página web y presione **F5** para volver a cargar la página web. Internet Explorer capturará el tráfico HTTP entre el explorador y el servidor web. La vista de resumen muestra todo el tráfico de red para una página:

![](tutorial-your-first-web-api/_static/image14.png)

Busque la entrada para el URI relativo "api/products /". Seleccione esta entrada y haga clic en **vaya a la vista detallada**. En la vista de detalles, hay pestañas para ver los encabezados de solicitud y respuesta y cuerpos. Por ejemplo, si hace clic en el **encabezados de solicitud** ficha, puede ver que el cliente solicitó &quot;application/json&quot; en el encabezado Accept.

![](tutorial-your-first-web-api/_static/image15.png)

Si hace clic en la ficha cuerpo de respuesta, puede ver cómo la lista de productos se serializa en JSON. Otros exploradores tienen una funcionalidad similar. Otra herramienta útil es [Fiddler](http://www.fiddler2.com/fiddler2/), un proxy de depuración web. Puede utilizar Fiddler para ver el tráfico HTTP y también para componer las solicitudes HTTP, que proporciona control total sobre los encabezados HTTP en la solicitud.

## <a name="see-this-app-running-on-azure"></a>Consulte esta aplicación se ejecuta en Azure

¿Desea ver el sitio terminado que se ejecuta como una aplicación web en directo? Puede implementar una versión completa de la aplicación en su cuenta de Azure, simplemente haga clic en el botón siguiente.

[![](http://azuredeploy.net/deploybutton.png)](https://deploy.azure.com/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/WebAPI-ProductsApp#/form/setup)

Necesita una cuenta de Azure para implementar esta solución en Azure. Si no dispone de una cuenta, tiene las siguientes opciones:

- [Abrir una cuenta de Azure de forma gratuita](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -obtiene crédito puede usar para probar los servicios de Azure de pago e incluso después de que se usan hasta, puede mantener la cuenta y usar servicios gratuitos de Azure.
- [Activar las ventajas de suscriptor MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -su suscripción a MSDN le proporciona crédito todos los meses que puede usar para servicios de Azure de pago.

## <a name="next-steps"></a>Pasos siguientes

- Para obtener un ejemplo más completo de un servicio HTTP que admite acciones POST, PUT y DELETE y escribe en una base de datos, vea [con Web API 2 con Entity Framework 6](../data/using-web-api-with-entity-framework/part-1.md).
- Para obtener más información sobre cómo crear aplicaciones web fluidas y sensibles encima de un servicio HTTP, consulte [aplicación de una única página ASP.NET](../../../single-page-application/index.md).
- Para obtener información sobre cómo implementar un proyecto web de Visual Studio en Azure App Service, consulte [crear una aplicación web ASP.NET en Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).
