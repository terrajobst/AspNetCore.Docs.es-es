---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-asp-net-health-monitoring-vb
title: "El registro de detalles del Error (VB) de supervisión de estado de ASP.NET | Documentos de Microsoft"
author: rick-anderson
description: "Sistema de supervisión de estado de Microsoft proporciona una manera fácil y personalizable para registrar varios eventos de web, incluidas las excepciones no controladas. Este tutorial le guía acc..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/09/2009
ms.topic: article
ms.assetid: 09a6c74e-936a-4c04-8547-5bb313a4e4a3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-asp-net-health-monitoring-vb
msc.type: authoredcontent
ms.openlocfilehash: 83f7504e3aeb02ed222712e7e51f612f7ffd5744
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2018
---
<a name="logging-error-details-with-aspnet-health-monitoring-vb"></a>Detalles de Error de registro con (VB) de supervisión de estado de ASP.NET
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](http://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_13_VB.zip) o [descarga de PDF](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial13_HealthMonitoring_vb.pdf)

> Sistema de supervisión de estado de Microsoft proporciona una manera fácil y personalizable para registrar varios eventos de web, incluidas las excepciones no controladas. Este tutorial le guía a través de configurar el sistema de supervisión de estado para iniciar las excepciones no controladas en una base de datos y para notificar a los desarrolladores a través de un mensaje de correo electrónico.


## <a name="introduction"></a>Introducción

El registro es una herramienta útil para supervisar el estado de una aplicación implementada y para diagnosticar los problemas que pueden surgir. Es especialmente importante registrar los errores que se producen en una aplicación implementada, por lo que puede resolverse. El `Error` evento se desencadena cuando se produce una excepción no controlada en una aplicación ASP.NET; la [tutorial anterior](processing-unhandled-exceptions-vb.md) se ha explicado cómo notificar a un desarrollador de un error y los detalles de registro mediante la creación de un controlador de eventos para el `Error` eventos. Sin embargo, creando un `Error` es necesario, el controlador de eventos para registrar los detalles del error y notifíquelo a un programador que esta tarea puede realizarse mediante ASP. De NET *sistema de supervisión de estado*.

El sistema de supervisión de estado se introdujo en ASP.NET 2.0 y está diseñado para supervisar el estado de una aplicación ASP.NET implementada registrando los eventos que se producen durante la vigencia de la solicitud o la aplicación. Los eventos registrados por el sistema de supervisión de estado se conocen como *eventos de supervisión de estado* o *Web eventos*e incluyen:

- Eventos de duración de la aplicación, por ejemplo, cuando una aplicación se inicia o detiene
- Eventos de seguridad, incluidos los intentos de inicio de sesión y errores de solicitud de autorización de dirección URL
- Errores de aplicación, incluidos los no controladas excepciones, análisis de excepciones, las excepciones de validación de solicitud y errores de compilación, entre otros tipos de errores de estado de vista.

Cuando un estado se desencadena el evento de supervisión se puede registrar en cualquier número de especificado *orígenes de registro*. El sistema de supervisión de estado se suministra con los orígenes de registro que registran eventos de Web en una base de datos de Microsoft SQL Server, o en el registro de eventos de Windows a través de un mensaje de correo electrónico, entre otros. También puede crear sus propios orígenes de registro.

Los eventos que registra el sistema de supervisión de estado, junto con los orígenes de registro utilizados, se definen en `Web.config`. Con unas pocas líneas de marcado de configuración sirve para registrar todas las excepciones no controladas en una base de datos y notificaciones de la excepción a través de correo electrónico de seguimiento de estado.

## <a name="exploring-the-health-monitoring-systems-configuration"></a>Explorar la configuración del sistema de supervisión de estado

El comportamiento del sistema de supervisión de estado se define por su información de configuración, que se encuentra en la [ `<healthMonitoring>` elemento](https://msdn.microsoft.com/library/2fwh2ss9.aspx) en `Web.config`. Esta sección de configuración define, entre otras cosas, las siguientes tres partes importantes de información:

1. Los eventos de supervisión de estado que, cuando se produce, se deben registrar,
2. Los orígenes de registro, y
3. ¿Cómo se asigna cada estado de supervisión de eventos definidos en (1) a los orígenes de registro definidos en (2).

Esta información se especifica a través de los elementos de configuración de tres nodos secundarios: [ `<eventMappings>` ](https://msdn.microsoft.com/library/yc5yk01w.aspx), [ `<providers>` ](https://msdn.microsoft.com/library/zaa41kz1.aspx), y [ `<rules>` ](https://msdn.microsoft.com/library/fe5wyxa0.aspx), respectivamente.

Información de configuración del sistema de supervisión de estado de forma predeterminada se encuentra en la `Web.config` en el archivo `%WINDIR%\Microsoft.NET\Framework\version\CONFIG` carpeta. Con algún marcado quitado para mayor brevedad, esta información de configuración de forma predeterminada, se muestra a continuación:

[!code-xml[Main](logging-error-details-with-asp-net-health-monitoring-vb/samples/sample1.xml)]

El estado de eventos de supervisión de interés se definen en el `<eventMappings>` elemento, que proporciona un nombre descriptivo humanos a una clase de eventos de supervisión de estado. En el código anterior, el `<eventMappings>` elemento asigna el nombre de lenguaje compatible con "todos los errores de" para el estado de supervisión de eventos de tipo `WebBaseErrorEvent` y el nombre "auditorías de errores" eventos de tipo de supervisión del estado `WebFailureAuditEvent`.

El `<providers>` elemento define los orígenes de registro, asignarles un nombre descriptivo humanos y especificar cualquier información de configuración específica del origen de registro. La primera `<add>` elemento define el proveedor "EventLogProvider", que registra el estado especificado supervisión de eventos mediante la `EventLogWebEventProvider` clase. La `EventLogWebEventProvider` clase registra el evento en el registro de eventos de Windows. El segundo `<add>` elemento define el proveedor "SqlWebEventProvider", que registra eventos en una base de datos de Microsoft SQL Server a través de la `SqlWebEventProvider` clase. La configuración de "SqlWebEventProvider" especifica la cadena de conexión de la base de datos (`connectionStringName`) entre otras opciones de configuración.

El `<rules>` elemento asigna los eventos especificados en el `<eventMappings>` elemento a orígenes de registro el `<providers>` elemento. De forma predeterminada, las aplicaciones web ASP.NET registrar todas las excepciones no controladas y auditoría de errores en el registro de eventos de Windows.

## <a name="logging-events-to-a-database"></a>Eventos de registro para una base de datos

Se pueden personalizar la configuración de predeterminada del sistema de supervisión de estado en una base de aplicación web a aplicación web mediante la adición de un `<healthMonitoring>` sección a la aplicación `Web.config` archivo. Puede incluir elementos adicionales en el `<eventMappings>`, `<providers>`, y `<rules>` secciones usando el `<add>` elemento. Para quitar una configuración de la utilización de la configuración predeterminada del `<remove>` elemento o use `<clear />` para quitar todos los valores predeterminados de una de estas secciones. Vamos a configurar la aplicación web de reseñas de libros para registrar las excepciones no controladas todos los en una base de datos de Microsoft SQL Server mediante la `SqlWebEventProvider` clase.

La `SqlWebEventProvider` clase forma parte del sistema de supervisión de estado y registra un evento en una base de datos de SQL Server especificada de supervisión de estado. El `SqlWebEventProvider` clase espera que la base de datos especificada incluye un procedimiento almacenado denominado `aspnet_WebEvent_LogEvent`. Este procedimiento almacenado se pasa los detalles del evento y se encarga de almacenar los detalles del evento. Lo bueno es que no es necesario crear este procedimiento almacenado procedimiento ni la tabla para almacenar los detalles del evento. Puede agregar estos objetos a la base de datos mediante el `aspnet_regsql.exe` herramienta.

> [!NOTE]
> El `aspnet_regsql.exe` herramienta tratada en la [ *configurar un sitio Web que utiliza servicios de aplicación* tutorial](configuring-a-website-that-uses-application-services-vb.md) cuando se agrega compatibilidad con ASP. Servicios de aplicaciones de red. Por lo tanto, base de datos del sitio Web de reseñas de libros ya contiene el `aspnet_WebEvent_LogEvent` procedimiento almacenado, que almacena la información de evento en una tabla denominada `aspnet_WebEvent_Events`.


Una vez que el procedimiento almacenado necesario y la tabla que se agrega a la base de datos, lo único que queda es indicar a para registrar todas las excepciones no controladas en la base de datos de supervisión de estado. Para ello, agregue el siguiente marcado de su sitio Web `Web.config` archivo:

[!code-xml[Main](logging-error-details-with-asp-net-health-monitoring-vb/samples/sample2.xml)]

El estado de supervisión de marcado de la configuración anterior se usa `<clear />` elementos que se va a borrar el estado predefinido información de configuración de supervisión del `<eventMappings>`, `<providers>`, y `<rules>` secciones. A continuación, se agrega una única entrada para cada una de estas secciones.

- El `<eventMappings>` elemento define un único evento de interés con el nombre "Todos los errores", que se desencadena cuando se produce una excepción no controlada de seguimiento de estado.
- El `<providers>` elemento define un origen de registro único denominado "SqlWebEventProvider" que usa el `SqlWebEventProvider` clase. El `connectionStringName` atributo se ha establecido en "ReviewsConnectionString", que es el nombre de la conexión de cadena definida en la `<connectionStrings>` sección.
- Por último, el &lt;reglas&gt; elemento indica que cuando ocurre un evento de "Todos los errores" que se debe registrar con el proveedor de "SqlWebEventProvider".

Esta información de configuración indica que el sistema para registrar todas las excepciones no controladas en la base de datos de reseñas de libros de supervisión de estado.

> [!NOTE]
> El `WebBaseErrorEvent` evento sólo se produce para errores del servidor; no se desencadena para los errores HTTP, como una solicitud para un recurso ASP.NET que no se encuentra. Esto difiere del comportamiento de la `HttpApplication` la clase `Error` evento, que se desencadena para el servidor y errores HTTP.


Para ver el estado del sistema en la acción de supervisión, visite el sitio Web y generar un error en tiempo de ejecución, visita `Genre.aspx?ID=foo`. Debería ver la página de error adecuado: la excepción detalles amarillo pantalla de muerte (cuando se visita localmente) o la página de error personalizada (al visitar el sitio de producción). En segundo plano, el sistema de supervisión de estado registra la información de error en la base de datos. Debe haber un registro en el `aspnet_WebEvent_Events` tabla (vea **figura 1**); este registro contiene información sobre el error en tiempo de ejecución que acaba de producirse.

[![](logging-error-details-with-asp-net-health-monitoring-vb/_static/image2.png)](logging-error-details-with-asp-net-health-monitoring-vb/_static/image1.png)

**Figura 1**: se registran los detalles del Error para el `aspnet_WebEvent_Events` tabla  
([Haga clic aquí para ver la imagen a tamaño completo](logging-error-details-with-asp-net-health-monitoring-vb/_static/image3.png))

### <a name="displaying-the-error-log-in-a-web-page"></a>Mostrar el registro de errores en una página Web

Con la configuración actual del sitio Web, el sistema de supervisión de estado registra todas las excepciones no controladas en la base de datos. Sin embargo, la supervisión de estado no proporciona ningún mecanismo para ver el registro de errores a través de una página web. Sin embargo, puede generar una página ASP.NET que muestra esta información de la base de datos. (Como veremos en breve, puede optar por tener los detalles del error que le enviados un mensaje de correo electrónico.)

Si crea una página de este tipo, asegúrese de que siga los pasos para permitir que sólo los usuarios autorizados ver los detalles del error. Si el sitio ya utiliza las cuentas de usuario, a continuación, puede usar las reglas de autorización de dirección URL para restringir el acceso a la página a determinados usuarios o roles. Para obtener más información sobre cómo conceder o restringir el acceso a páginas web en función de la sesión del usuario, consulte mi [tutoriales de seguridad del sitio Web](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md).

> [!NOTE]
> El tutorial posterior explora un sistema de registro y la notificación de error alternativas denominado ELMAH. ELMAH incluye un mecanismo integrado para ver el registro de errores de página web y como una fuente RSS.


## <a name="logging-events-to-email"></a>Registro de eventos al correo electrónico

El estado del sistema de supervisión incluye un proveedor de origen de registro "registra" un evento en un mensaje de correo electrónico. El origen del registro incluye la misma información que se registra en la base de datos en el cuerpo del mensaje de correo electrónico. Puede usar este origen de registro para notificar a un programador cuando se produce un determinado evento de supervisión de estado.

Vamos a actualizar la configuración del sitio Web para que se recibe un correo electrónico cada vez que una excepción se produce de reseñas de libros. Para lograr esto se debe realizar tres tareas:

1. Configurar la aplicación web ASP.NET para enviar correo electrónico. Esto se logra mediante la especificación de cómo se envían los mensajes de correo electrónico a través de la `<system.net>` elemento de configuración. Para obtener más información sobre el envío de correo electrónico Consulte mensajes en una aplicación ASP.NET [enviar correo electrónico en ASP.NET](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx) y [System.Net.Mail preguntas más frecuentes sobre](http://systemnetmail.com/).
2. Registrar el proveedor de origen del registro de correo electrónico en el `<providers>` elemento, y
3. Agregar una entrada a la `<rules>` elemento que se asigna al evento de "Todos los errores" al proveedor del registro de origen que agregó en el paso (2).

El sistema de supervisión de estado incluye dos clases de proveedor de origen de registro de correo electrónico: `SimpleMailWebEventProvider` y `TemplatedMailWebEventProvider`. El [ `SimpleMailWebEventProvider` clase](https://msdn.microsoft.com/library/system.web.management.simplemailwebeventprovider.aspx) envía un mensaje de correo electrónico de texto sin formato que incluya el evento detalla y ofrece personalización poco del cuerpo del correo electrónico. Con el [ `TemplatedMailWebEventProvider` clase](https://msdn.microsoft.com/library/system.web.management.templatedmailwebeventprovider.aspx) especificar una página ASP.NET cuyo marcado representado se utiliza como el cuerpo del mensaje de correo electrónico. El [ `TemplatedMailWebEventProvider` clase](https://msdn.microsoft.com/library/system.web.management.templatedmailwebeventprovider.aspx) ofrece mucho mayor control sobre el contenido y el formato de mensaje de correo electrónico, pero requiere un poco más trabajo por adelantado ya que tiene que crear la página ASP.NET que genera el cuerpo del mensaje de correo electrónico. Este tutorial se centra en usar la `SimpleMailWebEventProvider` clase.

Actualizar el estado sistema de supervisión `<providers>` elemento en el `Web.config` archivo para incluir un origen de registro para el `SimpleMailWebEventProvider` clase:

[!code-xml[Main](logging-error-details-with-asp-net-health-monitoring-vb/samples/sample3.xml)]

El marcado anterior utiliza la `SimpleMailWebEventProvider` clase como el proveedor de origen del registro y le asigna el nombre descriptivo "EmailWebEventProvider". Además, la `<add>` atributo incluye opciones de configuración adicionales, como el campo para y desde direcciones de mensaje de correo electrónico.

Con el origen del registro de correo electrónico definido, todo lo que queda es indicar el sistema para que utilice este origen para "registrar" excepciones no controladas de supervisión de estado. Esto se logra mediante la adición de una regla nueva en la `<rules>` sección:

[!code-xml[Main](logging-error-details-with-asp-net-health-monitoring-vb/samples/sample4.xml)]

El `<rules>` sección ahora incluye dos reglas. La primera de ellas, con el nombre "Todos los errores a correo electrónico", envía todas las excepciones no controladas en el origen del registro "EmailWebEventProvider". Esta regla tiene el efecto de enviar los detalles sobre los errores en el sitio Web a los especificados para la dirección. La regla de "Todos los errores a base de datos" registra los detalles del error en la base de datos del sitio. Por lo tanto, cada vez que produce una excepción no controlada en el sitio de los detalles están registrados en la base de datos y envía a la dirección de correo electrónico especificada.

**Figura 2** muestra el correo electrónico generado por el `SimpleMailWebEventProvider` clase al visitar `Genre.aspx?ID=foo`.

[![](logging-error-details-with-asp-net-health-monitoring-vb/_static/image5.png)](logging-error-details-with-asp-net-health-monitoring-vb/_static/image4.png)

**Figura 2**: los detalles del Error se envían en un mensaje de correo electrónico  
([Haga clic aquí para ver la imagen a tamaño completo](logging-error-details-with-asp-net-health-monitoring-vb/_static/image6.png))

## <a name="summary"></a>Resumen

El sistema de supervisión de estado ASP.NET está diseñado para permitir a los administradores a supervisar el estado de una aplicación web implementadas. Eventos de supervisión de estado se generan cuando expandir determinadas acciones, como cuando se detiene la aplicación, cuando un usuario inicia sesión correctamente en el sitio, o cuando se produce una excepción no controlada. Estos eventos se pueden registrar a cualquier número de orígenes de registro. Este tutorial se ha explicado cómo registrar los detalles de las excepciones no controladas en una base de datos y a través de un mensaje de correo electrónico.

Este tutorial se centra en el uso para registrar excepciones no controladas, pero tenga en cuenta que la supervisión de estado está diseñada para medir el estado general de una aplicación ASP.NET implementada e incluye un gran número de eventos de supervisión de estado y no los orígenes de registro de seguimiento de estado explorar aquí. ¿Qué es más, puede crear su propios eventos y orígenes de registro de seguimiento de estado fuese necesario surgen. Si está interesado en aprender más acerca de la supervisión de estado, es un buen primer paso leer [Erik Reitan](https://blogs.msdn.com/erikreitan/archive/2006/05/22/603586.aspx)del [preguntas más frecuentes de supervisión de estado](https://blogs.msdn.com/erikreitan/archive/2006/05/22/603586.aspx). A continuación, consulte [How To: Use supervisión de estado en ASP.NET 2.0](https://msdn.microsoft.com/library/ms998306.aspx).

Feliz programación.

### <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas tratados en este tutorial, consulte los siguientes recursos:

- [Información general de supervisión de estado de ASP.NET](https://msdn.microsoft.com/library/bb398933.aspx)
- [Configurar y personalizar el sistema de ASP.NET de supervisión de estado](http://dotnetslackers.com/articles/aspnet/ConfiguringAndCustomizingTheHealthMonitoringSystemOfASPNET.aspx)
- [Preguntas más frecuentes: estado de supervisión en ASP.NET 2.0](https://blogs.msdn.com/erikreitan/archive/2006/05/22/603586.aspx)
- [Cómo: Enviar correo electrónico para las notificaciones de supervisión de estado](https://msdn.microsoft.com/library/ms227553.aspx)
- [Cómo: Usar la supervisión de estado en ASP.NET](https://msdn.microsoft.com/library/ms998306.aspx)
- [Estado de supervisión en ASP.NET](http://aspnet.4guysfromrolla.com/articles/031407-1.aspx)

>[!div class="step-by-step"]
[Anterior](processing-unhandled-exceptions-vb.md)
[Siguiente](logging-error-details-with-elmah-vb.md)
