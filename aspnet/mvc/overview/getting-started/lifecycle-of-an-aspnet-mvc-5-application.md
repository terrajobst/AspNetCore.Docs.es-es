---
uid: mvc/overview/getting-started/lifecycle-of-an-aspnet-mvc-5-application
title: Ciclo de vida de una aplicación ASP.NET MVC 5 | Microsoft Docs
author: cephalin
description: Descargue un documento PDF que muestra el ciclo de vida de una aplicación ASP.NET MVC 5. Este documento del ciclo de vida proporciona una visión general del ciclo de vida MVC un...
ms.author: aspnetcontent
ms.date: 02/28/2014
ms.assetid: 9c1e3a75-b644-4480-8326-11300b1ec4b3
msc.legacyurl: /mvc/overview/getting-started/lifecycle-of-an-aspnet-mvc-5-application
msc.type: authoredcontent
ms.openlocfilehash: cceaa377e77a7229edb2e33a67d000e26b43358a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37839260"
---
<a name="lifecycle-of-an-aspnet-mvc-5-application"></a>Ciclo de vida de una aplicación ASP.NET MVC 5
====================
por [Cephas Lin](https://github.com/cephalin)

[Descargue el documento PDF](lifecycle-of-an-aspnet-mvc-5-application/_static/lifecycle-of-an-aspnet-mvc-5-application1.pdf)

Aquí puede descargar un documento PDF que realizar una copia de los gráficos del ciclo de vida de cada aplicación de ASP.NET MVC 5, de recepción HTTP de solicitud para enviar la respuesta HTTP al cliente. Se ha diseñado como una herramienta educativa para aquellos que está familiarizado con ASP.NET MVC y también como una referencia para los usuarios que necesitan para profundizar en los aspectos específicos de la aplicación. El documento PDF tiene las siguientes características:

- Pertinentes [HttpApplication](https://msdn.microsoft.com/library/system.web.httpapplication.aspx) fases que le ayudarán a comprender dónde MVC se integra en el [ciclo de vida de aplicaciones de ASP.NET](https://msdn.microsoft.com/library/bb470252.aspx).
- Una visión general del ciclo de vida de aplicación MVC, donde podrá identificar las principales fases que todas las aplicaciones MVC pasa a través de la canalización de procesamiento de solicitudes.  
    ![](lifecycle-of-an-aspnet-mvc-5-application/_static/image1.jpg)
- Una vista de detalle que muestra maniobras hacia abajo en los detalles de la canalización de procesamiento de la solicitud. Puede comparar la vista de alto nivel y la vista de detalles para ver cómo se recopilan los detalles de los ciclos de vida en las distintas fases. [Descargar PDF](lifecycle-of-an-aspnet-mvc-5-application/_static/lifecycle-of-an-aspnet-mvc-5-application1.pdf) para ver una vista más grande.
    ![](lifecycle-of-an-aspnet-mvc-5-application/_static/image2.jpg)
- Selección de ubicación y el propósito de todos los métodos reemplazables en el [controlador](https://msdn.microsoft.com/library/system.web.mvc.controller.aspx) objeto en la canalización de procesamiento de solicitudes. Puede o no tener la necesidad de reemplazar un método de prueba, pero es importante comprender su rol en el ciclo de vida de la aplicación, por lo que puede escribir código en la fase del ciclo de vida adecuado para el efecto deseado.
- Diagramas de corriente de copia de seguridad que muestra cómo cada uno de los tipos de filtro (autenticación, autorización, acción y resultados) se invoca.
- Vincular a un artículo útil o el blog de cada punto de interés en la vista de detalle.


## <a name="next-steps"></a>Pasos siguientes

¿Este documento satisface sus necesidades? Agradecemos sus comentarios. Si tiene alguna duda sobre el ciclo de vida de ASP.NET MVC en la aplicación, [Stackoverflow](http://stackoverflow.com/help) y [foros de ASP.NET MVC](https://forums.asp.net/1146.aspx) son lugares excelentes para preguntar. Siga [me](https://twitter.com/Cephas_MSFT) en twitter para que pueda obtener actualizaciones en mi tutoriales más recientes.
