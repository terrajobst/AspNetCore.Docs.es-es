---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-asp-net-health-monitoring-cs
title: Registrar detalles de Error con el estado de ASP.NET de supervisión (C#) | Microsoft Docs
author: rick-anderson
description: Sistema de supervisión de estado de Microsoft proporciona una manera fácil y personalizable para iniciar varios eventos de web, incluidos las excepciones no controladas. Este tutorial le guía acc...
ms.author: riande
ms.date: 06/09/2009
ms.assetid: b1abb452-642a-4ff3-8504-37b85590ff79
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-asp-net-health-monitoring-cs
msc.type: authoredcontent
ms.openlocfilehash: 32f78927ae3e3f90392fea5968a035ab30c423ac
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828221"
---
<a name="logging-error-details-with-aspnet-health-monitoring-c"></a>Registrar detalles de Error con el estado de ASP.NET de supervisión (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](http://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_13_CS.zip) o [descargar PDF](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial13_HealthMonitoring_cs.pdf)

> Sistema de supervisión de estado de Microsoft proporciona una manera fácil y personalizable para iniciar varios eventos de web, incluidos las excepciones no controladas. Este tutorial le guía a través de la supervisión del sistema de estado para iniciar las excepciones no controladas en una base de datos y para notificar a los desarrolladores a través de un mensaje de correo electrónico.


## <a name="introduction"></a>Introducción

El registro es una herramienta útil para supervisar el estado de una aplicación implementada y para diagnosticar los problemas que puedan surgir. Es especialmente importante registrar los errores que se producen en una aplicación implementada, por lo que puede resolverse. El `Error` evento se desencadena cuando se produce una excepción no controlada en una aplicación ASP.NET; el [tutorial anterior](processing-unhandled-exceptions-cs.md) se ha explicado cómo notificar a un desarrollador de un error y sus detalles de registro mediante la creación de un controlador de eventos para el `Error` eventos. Sin embargo, creando un `Error` controlador de eventos para registrar los detalles del error y notificar a un desarrollador no es necesario, como ASP puede realizar esta tarea. La red *del sistema de supervisión de estado*.

El sistema de supervisión de estado se introdujo en ASP.NET 2.0 y está diseñado para supervisar el estado de una aplicación de ASP.NET implementada por los eventos de registro que se producen durante la vigencia de la solicitud o la aplicación. Los eventos registrados por el sistema de supervisión de estado se conocen como *eventos de supervisión de estado* o *Web eventos*e incluyen:

- Eventos de duración de la aplicación, por ejemplo, cuando se inicia o detiene una aplicación
- Eventos de seguridad, incluidos los intentos de inicio de sesión y no pudo las solicitudes de autorización de URL
- Errores de aplicación, incluidos no controlan de excepciones, excepciones, las excepciones de validación de solicitudes y errores de compilación, entre otros tipos de errores de análisis de estado de vista.

Cuando se provoca el evento de supervisión de un estado de la se puede registrar en cualquier número de especificado *orígenes de registro*. El sistema de supervisión de estado se incluye con los orígenes de registro que inician eventos Web en una base de datos de Microsoft SQL Server, o en el registro de eventos de Windows a través de un mensaje de correo electrónico, entre otros. También puede crear sus propios orígenes de registro.

Los eventos que se inicia el sistema de supervisión de estado, junto con los orígenes de registro utilizados, se definen en `Web.config`. Con unas pocas líneas de marcado de la configuración puede usar para iniciar todas las excepciones no controladas en una base de datos y para avisarle de la excepción a través de correo electrónico de seguimiento de estado.

## <a name="exploring-the-health-monitoring-systems-configuration"></a>Exploración de la configuración del sistema de supervisión de estado

El comportamiento del sistema de supervisión de estado se define por su información de configuración, que se encuentra en la [ `<healthMonitoring>` elemento](https://msdn.microsoft.com/library/2fwh2ss9.aspx) en `Web.config`. Esta sección de configuración define, entre otras cosas, las siguientes tres partes importantes de información:

1. Los eventos de supervisión de estado que, cuando se genera, debe haber iniciado sesión,
2. Los orígenes de registro, y
3. Cómo se asigna cada evento definido en (1) la supervisión de estado a los orígenes de registro definidos en (2).

Esta información se especifica a través de los elementos de configuración de tres nodos secundarios: [ `<eventMappings>` ](https://msdn.microsoft.com/library/yc5yk01w.aspx), [ `<providers>` ](https://msdn.microsoft.com/library/zaa41kz1.aspx), y [ `<rules>` ](https://msdn.microsoft.com/library/fe5wyxa0.aspx), respectivamente.

Puede encontrar la información de configuración del sistema de supervisión de estado predeterminada en el `Web.config` archivo `%WINDIR%\Microsoft.NET\Framework\version\CONFIG` carpeta. Esta información de configuración predeterminado, con algún marcado quitado para mayor brevedad, se muestra a continuación:

[!code-xml[Main](logging-error-details-with-asp-net-health-monitoring-cs/samples/sample1.xml)]

El estado de supervisión de eventos de interés se definen en el `<eventMappings>` elemento, que proporciona un nombre fácil de usar en una clase de eventos de supervisión de estado. En el marcado anterior, el `<eventMappings>` elemento asigna el nombre de fácil de usar "Todos los errores" para el estado de supervisión de eventos de tipo `WebBaseErrorEvent` y el nombre "auditorías de errores" en los eventos de tipo de supervisión de estado `WebFailureAuditEvent`.

El `<providers>` elemento define los orígenes de registro, asignarles un nombre fácil de usar y especificar cualquier información de configuración específicas del origen de registro. La primera `<add>` elemento define el proveedor "EventLogProvider", que registra el estado especificado, supervisión de eventos mediante el `EventLogWebEventProvider` clase. La `EventLogWebEventProvider` clase registra el evento al registro de eventos de Windows. El segundo `<add>` elemento define el proveedor "SqlWebEventProvider", que registra eventos en una base de datos de Microsoft SQL Server a través de la `SqlWebEventProvider` clase. La configuración de "SqlWebEventProvider" especifica la cadena de conexión de la base de datos (`connectionStringName`) entre otras opciones de configuración.

El `<rules>` los eventos especificados en el elemento asigna el `<eventMappings>` elemento a orígenes de registro el `<providers>` elemento. De forma predeterminada, las aplicaciones web ASP.NET registrar todas las excepciones no controladas y auditoría de errores en el registro de eventos de Windows.

## <a name="logging-events-to-a-database"></a>Eventos de registro para una base de datos

Se puede personalizar la configuración de predeterminada del sistema de supervisión de estado en una base de la aplicación web a aplicación web mediante la adición de un `<healthMonitoring>` sección a la aplicación `Web.config` archivo. Puede incluir elementos adicionales en el `<eventMappings>`, `<providers>`, y `<rules>` secciones utilizando el `<add>` elemento. Para quitar una configuración de la configuración predeterminada, use el `<remove>` elemento, o use `<clear />` para quitar todos los valores predeterminados de una de estas secciones. Vamos a configurar la aplicación web de reseñas de libros para iniciar las excepciones no controladas todo en una base de datos de Microsoft SQL Server mediante el `SqlWebEventProvider` clase.

La `SqlWebEventProvider` clase forma parte del sistema de supervisión de estado y registra un evento a una base de datos de SQL Server especificada de supervisión de estado. El `SqlWebEventProvider` clase espera que la base de datos especificada incluye un procedimiento almacenado denominado `aspnet_WebEvent_LogEvent`. Este procedimiento almacenado se pasa los detalles del evento y se encarga de almacenar los detalles del evento. La buena noticia es que no es necesario crear este procedimiento almacenado procedimiento ni la tabla para almacenar los detalles del evento. Puede agregar estos objetos a la base de datos mediante el `aspnet_regsql.exe` herramienta.

> [!NOTE]
> El `aspnet_regsql.exe` herramienta explicada en el [ *configurar un sitio Web que usa servicios de aplicación* tutorial](configuring-a-website-that-uses-application-services-cs.md) cuando se ha agregado compatibilidad con ASP. Servicios de aplicación de la red. Por lo tanto, base de datos del sitio Web de reseñas de libros ya contiene el `aspnet_WebEvent_LogEvent` procedimiento almacenado, que almacena la información de evento en una tabla denominada `aspnet_WebEvent_Events`.


Una vez que el procedimiento almacenado necesario y la tabla que se agrega a la base de datos, todo lo que queda es indicar a para registrar todas las excepciones no controladas en la base de datos de supervisión de estado. Esto se consigue agregando el siguiente marcado para su sitio Web `Web.config` archivo:

[!code-xml[Main](logging-error-details-with-asp-net-health-monitoring-cs/samples/sample2.xml)]

El marcado de la configuración anterior se usa de supervisión de estado `<clear />` elementos que se va a borrar la información de configuración de supervisión de estado predefinido la `<eventMappings>`, `<providers>`, y `<rules>` secciones. A continuación, se agrega una entrada única para cada una de estas secciones.

- El `<eventMappings>` elemento define un único evento de interés denominado "Todos los errores", que se desencadena cuando se produce una excepción no controlada de supervisión de estado.
- El `<providers>` elemento define un origen de registro único denominado "SqlWebEventProvider" que usa el `SqlWebEventProvider` clase. El `connectionStringName` atributo se ha establecido en "ReviewsConnectionString", que es el nombre de conexión de nuestra cadena definida en la `<connectionStrings>` sección.
- Por último, el &lt;reglas&gt; elemento indica que cuando ocurre un evento de "Todos los errores" que se debe registrar con el proveedor "SqlWebEventProvider".

Esta información de configuración indica el estado de supervisión del sistema para registrar todas las excepciones no controladas en la base de datos de reseñas de libros.

> [!NOTE]
> El `WebBaseErrorEvent` evento se desencadena solo para errores de servidor; no se genera para los errores HTTP, como una solicitud de un recurso de ASP.NET que no se encuentra. Esto difiere del comportamiento de la `HttpApplication` la clase `Error` evento, que se desencadena para el servidor y errores HTTP.


Para ver el estado del sistema en la acción de supervisión, visite el sitio Web y generan un error en tiempo de ejecución, visite `Genre.aspx?ID=foo`. Debería ver la página de error adecuado: la excepción detalles amarillo pantalla de la muerte (cuando se visita localmente) o en la página de error personalizada (cuando se visita el sitio de producción). En segundo plano, el sistema de supervisión de estado registra la información de error en la base de datos. Debe haber un registro de la `aspnet_WebEvent_Events` tabla (consulte **figura 1**); este registro contiene información sobre el error en tiempo de ejecución que acaba de producirse.

[![](logging-error-details-with-asp-net-health-monitoring-cs/_static/image2.png)](logging-error-details-with-asp-net-health-monitoring-cs/_static/image1.png)

**Figura 1**: se registraron los detalles del Error para el `aspnet_WebEvent_Events` tabla  
([Haga clic aquí para ver imagen en tamaño completo](logging-error-details-with-asp-net-health-monitoring-cs/_static/image3.png))

### <a name="displaying-the-error-log-in-a-web-page"></a>Mostrar el registro de errores en una página Web

Con la configuración actual del sitio Web del sistema de supervisión de estado registra todas las excepciones no controladas en la base de datos. Sin embargo, la supervisión de estado no proporciona ningún mecanismo para ver el registro de errores a través de una página web. Sin embargo, podría crear una página ASP.NET que muestra esta información de la base de datos. (Como veremos en breve, puede optar por tener los detalles del error enviados en un mensaje de correo electrónico.)

Si crea una página, asegúrese de que siga los pasos para permitir que solo los usuarios autorizados ver los detalles del error. Si el sitio ya emplea las cuentas de usuario, a continuación, puede usar las reglas de autorización de dirección URL para restringir el acceso a la página a determinados usuarios o roles. Para obtener más información sobre cómo conceder o restringir el acceso a las páginas web en función del usuario que ha iniciado sesión, consulte mi [tutoriales de seguridad del sitio Web](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md).

> [!NOTE]
> El tutorial posterior explora un sistema de registro y notificación de error alternativas denominado ELMAH. ELMAH incluye un mecanismo integrado para ver el registro de errores de página web y como una fuente RSS.


## <a name="logging-events-to-email"></a>Registro de eventos al correo electrónico

El estado del sistema de supervisión incluye un proveedor de origen de registro que se "registra" un evento en un mensaje de correo electrónico. El origen del registro incluye la misma información que se registra en la base de datos en el cuerpo del mensaje de correo electrónico. Puede usar este origen de registro para notificar a un desarrollador cuando se produce un determinado evento de supervisión de estado.

Vamos a actualizar la configuración del sitio Web para que se reciba un correo electrónico cada vez que una excepción se produce de reseñas de libros. Para lograr esto se debe realizar tres tareas:

1. Configurar la aplicación web ASP.NET para enviar correo electrónico. Esto se logra mediante la especificación de cómo se envían los mensajes de correo electrónico a través de la `<system.net>` elemento de configuración. Para obtener más información sobre el envío de correo electrónico Consulte mensajes en una aplicación ASP.NET [enviar correo electrónico en ASP.NET](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx) y [System.Net.Mail preguntas más frecuentes sobre](http://systemnetmail.com/).
2. Registrar el proveedor de origen del registro de correo electrónico en el `<providers>` elemento, y
3. Agregar una entrada a la `<rules>` elemento que se asigna el evento "Todos los errores" para el proveedor de origen de registro que agregó en el paso (2).

El sistema de supervisión de estado incluye dos clases de proveedor del origen de registro de correo electrónico: `SimpleMailWebEventProvider` y `TemplatedMailWebEventProvider`. El [ `SimpleMailWebEventProvider` clase](https://msdn.microsoft.com/library/system.web.management.simplemailwebeventprovider.aspx) envía un mensaje de correo electrónico de texto sin formato que incluya el evento detalla y proporciona poca personalización del cuerpo del correo electrónico. Con el [ `TemplatedMailWebEventProvider` clase](https://msdn.microsoft.com/library/system.web.management.templatedmailwebeventprovider.aspx) especificar una página ASP.NET cuyo marcado representado se utiliza como el cuerpo del mensaje de correo electrónico. El [ `TemplatedMailWebEventProvider` clase](https://msdn.microsoft.com/library/system.web.management.templatedmailwebeventprovider.aspx) le ofrece un control mucho mayor sobre el contenido y el formato de mensaje de correo electrónico, pero requiere un poco más trabajo por adelantado ya que tiene que crear la página ASP.NET que genera el cuerpo del mensaje de correo electrónico. En este tutorial se centra en usar la `SimpleMailWebEventProvider` clase.

Actualizar el estado sistema de supervisión `<providers>` elemento en el `Web.config` archivo para incluir un origen de registro para el `SimpleMailWebEventProvider` clase:

[!code-xml[Main](logging-error-details-with-asp-net-health-monitoring-cs/samples/sample3.xml)]

El marcado anterior usa la `SimpleMailWebEventProvider` clase como el proveedor de origen del registro y le asigna el nombre descriptivo "EmailWebEventProvider". Además, el `<add>` atributo incluye opciones de configuración adicionales, como la línea y de las direcciones de mensaje de correo electrónico.

Con el origen de registro de correo electrónico que se define, todo lo que queda es indicar el estado de supervisión del sistema para usar este origen de las excepciones no controladas "iniciar". Esto se logra agregando una nueva regla en el `<rules>` sección:

[!code-xml[Main](logging-error-details-with-asp-net-health-monitoring-cs/samples/sample4.xml)]

El `<rules>` sección ahora incluye dos reglas. La primera de ellas, denominada "Todos los errores a Email", envía todas las excepciones no controladas en el origen del registro "EmailWebEventProvider". Esta regla tiene el efecto de enviar información sobre los errores en el sitio Web especificado a la dirección. La regla de "Todos los errores a base de datos" registra los detalles del error en la base de datos de la carpeta del sitio. Por lo tanto, cada vez que produce una excepción no controlada en el sitio de sus detalles se registran en la base de datos tanto enviados a la dirección de correo electrónico especificada.

**Figura 2** muestra el correo electrónico generado por el `SimpleMailWebEventProvider` clase cuando se visita `Genre.aspx?ID=foo`.

[![](logging-error-details-with-asp-net-health-monitoring-cs/_static/image5.png)](logging-error-details-with-asp-net-health-monitoring-cs/_static/image4.png)

**Figura 2**: los detalles del Error se envían en un mensaje de correo electrónico  
([Haga clic aquí para ver imagen en tamaño completo](logging-error-details-with-asp-net-health-monitoring-cs/_static/image6.png))

## <a name="summary"></a>Resumen

El sistema de supervisión de estado ASP.NET está diseñado para permitir que los administradores a supervisar el estado de una aplicación web implementada. Eventos de supervisión de estado se generan cuando unfold determinadas acciones, como cuando se detiene la aplicación, cuando un usuario inicia sesión correctamente en el sitio, o cuando se produce una excepción no controlada. Estos eventos se pueden registrar en cualquier número de orígenes de registro. En este tutorial se ha explicado cómo registrar los detalles de las excepciones no controladas en una base de datos y a través de un mensaje de correo electrónico.

En este tutorial se centra en el uso para registrar excepciones no controladas, pero tenga en cuenta que la supervisión de estado está diseñada para medir el estado general de una aplicación de ASP.NET implementada e incluye una gran cantidad de eventos de supervisión de estado y no los orígenes de registro de seguimiento de estado analizado. ¿Qué es más, puede crear su propios eventos y orígenes de registro de seguimiento de estado debe la necesidad de surgir. Si está interesado en aprender más sobre la supervisión de estado, es un buen primer paso leer [Erik Reitan](https://blogs.msdn.com/erikreitan/archive/2006/05/22/603586.aspx)del [preguntas más frecuentes sobre la supervisión de estado](https://blogs.msdn.com/erikreitan/archive/2006/05/22/603586.aspx). A continuación, consulte [How To: Use supervisión del estado en ASP.NET 2.0](https://msdn.microsoft.com/library/ms998306.aspx).

Feliz programación.

### <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas tratados en este tutorial, consulte los siguientes recursos:

- [Información general de supervisión de estado de ASP.NET](https://msdn.microsoft.com/library/bb398933.aspx)
- [Configurar y personalizar el sistema de ASP.NET de supervisión de estado](http://dotnetslackers.com/articles/aspnet/ConfiguringAndCustomizingTheHealthMonitoringSystemOfASPNET.aspx)
- [Preguntas más frecuentes: supervisión del estado en ASP.NET 2.0](https://blogs.msdn.com/erikreitan/archive/2006/05/22/603586.aspx)
- [Cómo: Enviar correo electrónico para notificaciones de supervisión de estado](https://msdn.microsoft.com/library/ms227553.aspx)
- [Cómo: Usar la supervisión del estado en ASP.NET](https://msdn.microsoft.com/library/ms998306.aspx)
- [Supervisión del estado en ASP.NET](http://aspnet.4guysfromrolla.com/articles/031407-1.aspx)

> [!div class="step-by-step"]
> [Anterior](processing-unhandled-exceptions-cs.md)
> [Siguiente](logging-error-details-with-elmah-cs.md)
