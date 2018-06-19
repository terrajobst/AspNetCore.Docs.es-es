---
uid: mvc/overview/getting-started/lifecycle-of-an-aspnet-mvc-5-application
title: Ciclo de vida de una aplicación ASP.NET MVC 5 | Documentos de Microsoft
author: cephalin
description: Descargar un documento PDF que muestra el ciclo de vida de una aplicación ASP.NET MVC 5. Este documento del ciclo de vida proporciona una vista de alto nivel del ciclo de vida MVC un...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/28/2014
ms.topic: article
ms.assetid: 9c1e3a75-b644-4480-8326-11300b1ec4b3
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/lifecycle-of-an-aspnet-mvc-5-application
msc.type: authoredcontent
ms.openlocfilehash: 50d58d10c11677fa72ede6a03e686cbde4cbae1d
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
ms.locfileid: "28036497"
---
<a name="lifecycle-of-an-aspnet-mvc-5-application"></a>Ciclo de vida de una aplicación de MVC de ASP.NET 5
====================
por [Cephas Lin](https://github.com/cephalin)

[Descargue el documento PDF](lifecycle-of-an-aspnet-mvc-5-application/_static/lifecycle-of-an-aspnet-mvc-5-application1.pdf)

Aquí puede descargar un documento PDF que solicitar el ciclo de vida de cada aplicación de ASP.NET MVC 5, de recepción de HTTP para enviar la respuesta HTTP de gráficos que se devuelva al cliente. Se ha diseñado como una herramienta educativa para aquellos que está familiarizado con ASP.NET MVC y también como una referencia para aquellos que necesitan para profundizar en los aspectos específicos de la aplicación. El documento PDF tiene las siguientes características:

- Relevante [HttpApplication](https://msdn.microsoft.com/library/system.web.httpapplication.aspx) fases para ayudarle a entender dónde MVC se integra en el [ciclo de vida de aplicación de ASP.NET](https://msdn.microsoft.com/library/bb470252.aspx).
- Una vista de alto nivel del ciclo de vida de la aplicación de MVC, donde puede entender las fases principales que todas las aplicaciones MVC pasa a través de la canalización de procesamiento de la solicitud.  
    ![](lifecycle-of-an-aspnet-mvc-5-application/_static/image1.jpg)
- Una vista de detalle que muestra aumenta hacia abajo en los detalles de la canalización de procesamiento de la solicitud. Puede comparar la vista de alto nivel y la vista de detalle para ver cómo se recopilan los detalles de los ciclos de vida en las distintas fases. [Descarga de PDF](lifecycle-of-an-aspnet-mvc-5-application/_static/lifecycle-of-an-aspnet-mvc-5-application1.pdf) para ver una vista más grande.
    ![](lifecycle-of-an-aspnet-mvc-5-application/_static/image2.jpg)
- Selección de ubicación y finalidad de todos los métodos reemplazables en el [controlador](https://msdn.microsoft.com/library/system.web.mvc.controller.aspx) objeto en la canalización de procesamiento de solicitudes. Puede o no puede tener la necesidad de reemplazar un método de prueba, pero es importante para comprender su rol en el ciclo de vida de la aplicación para que pueda escribir código en la fase del ciclo de vida adecuado para el efecto deseado.
- Diagramas de arriba inyectado que muestra cómo cada uno de los tipos de filtro (autenticación, autorización, acción y resultados) se invoca.
- Vincular a un artículo útil o el blog de cada punto de interés en la vista de detalle.


## <a name="next-steps"></a>Pasos siguientes

¿Este documento satisface sus necesidades? Agradecemos sus comentarios. Si tiene alguna duda sobre el ciclo de vida de ASP.NET MVC en la aplicación, [Stackoverflow](http://stackoverflow.com/help) y [foros de ASP.NET MVC](https://forums.asp.net/1146.aspx) son lugares excelentes que ponerse en contacto. Siga [me](https://twitter.com/Cephas_MSFT) en twitter, por lo que puede obtener actualizaciones en mi tutoriales más recientes.
