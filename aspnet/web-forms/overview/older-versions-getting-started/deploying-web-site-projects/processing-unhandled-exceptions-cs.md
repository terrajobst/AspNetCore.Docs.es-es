---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/processing-unhandled-exceptions-cs
title: Procesar las excepciones no controladas (C#) | Documentos de Microsoft
author: rick-anderson
description: Cuando se produce un error en tiempo de ejecución en una aplicación web en producción es importante para notificar a un desarrollador y registre el error para que se pueden diagnosticar en una la...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/09/2009
ms.topic: article
ms.assetid: 5bc1afd5-2484-4528-b158-ab218ba150e8
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/processing-unhandled-exceptions-cs
msc.type: authoredcontent
ms.openlocfilehash: fccd529f5c489ed28f3cb01f07ffdc4452ead36d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="processing-unhandled-exceptions-c"></a>Procesar las excepciones no controladas (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnet/web-forms/overview/older-versions-getting-started/deploying-web-site-projects/processing-unhandled-exceptions-cs/samples) ([cómo descargarlo](/aspnet/core/tutorials/index#how-to-download-a-sample))

> Cuando se produce un error en tiempo de ejecución en una aplicación web en producción es importante para notificar a un desarrollador y registre el error para que se pueden diagnosticar en un momento posterior en el tiempo. Este tutorial proporciona una visión general del proceso de ASP.NET procesa los errores en tiempo de ejecución y se examinan una manera de tener código personalizado se ejecute cada vez que un burbujas de excepción no controlada al tiempo de ejecución de ASP.NET.


## <a name="introduction"></a>Introducción

Cuando se produce una excepción no controlada en una aplicación ASP.NET, propaga hasta el tiempo de ejecución ASP.NET, que genera el `Error` eventos y muestra la página de error adecuado. Hay tres tipos diferentes de las páginas de error: el tiempo de ejecución Error amarillo pantalla de muerte (YSOD); los detalles de excepción YSOD; y páginas de errores personalizados. En el [tutorial anterior](displaying-a-custom-error-page-cs.md) hemos configurado la aplicación para utilizar una página de error personalizada para los usuarios remotos y la YSOD de detalles de excepción para los usuarios que visitan de forma local.

Mediante una página de error personalizado sencillo humanos que coincide con la apariencia y funcionamiento del sitio es preferible a la predeterminada YSOD de Error de tiempo de ejecución, pero mostrar una página de error personalizada es solo una parte de una completa solución de control de errores. Cuando se produce un error en una aplicación en producción, es importante que los desarrolladores se notificación el error para que puedan desenterrar la causa de la excepción y solucionarlo. Además, se deben registrar los detalles del error para que el error se puede examinar y diagnosticar en un momento posterior en el tiempo.

Este tutorial muestra cómo obtener acceso a los detalles de una excepción no controlada para que se pueden registrar y un programador de una notificación. Los dos tutoriales siguiendo este exploran las bibliotecas de registro de errores que, después de un poco de configuración, notificar a los desarrolladores de errores en tiempo de ejecución y sus detalles de registro automáticamente.

> [!NOTE]
> La información que se examinan en este tutorial es muy útil si tiene que procesar las excepciones no controladas de alguna manera único o personalizada. En casos en que solo tenga que registrar la excepción y notificar a un desarrollador, usando una biblioteca de registro de error es la mejor opción. Los siguientes dos tutoriales proporcionan una visión general de dos de estas bibliotecas.


## <a name="executing-code-when-theerrorevent-is-raised"></a>Ejecutar código cuando el`Error`evento se desencadena

Eventos proporcionan un mecanismo para señalar que se ha producido algo interesante y otro objeto que se va a ejecutar el código en respuesta de un objeto. Como desarrollador de ASP.NET está acostumbrado a pensar en eventos. Si desea ejecutar el código cuando el visitante hace clic en un botón determinado, crear un controlador de eventos para ese botón `Click` eventos y coloque el código no existe. Dado que el tiempo de ejecución ASP.NET genera su [ `Error` eventos](https://msdn.microsoft.com/library/system.web.httpapplication.error.aspx) siempre que se produce una excepción no controlada, se supone que el código para registrar los detalles del error saldría en un controlador de eventos. Pero ¿cómo se crea un controlador de eventos para el `Error` eventos?

El `Error` evento es uno de muchos eventos en el [ `HttpApplication` clase](https://msdn.microsoft.com/library/system.web.httpapplication.aspx) que se generan en determinadas fases de la canalización HTTP durante la duración de una solicitud. Por ejemplo, el `HttpApplication` la clase [ `BeginRequest` eventos](https://msdn.microsoft.com/library/system.web.httpapplication.beginrequest.aspx) se desencadena al comienzo de cada solicitud; su [ `AuthenticateRequest` evento](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx) se genera cuando un módulo de seguridad ha identificado el solicitante. Estos `HttpApplication` eventos dan al desarrollador de la página a fin de ejecutar una lógica personalizada en varios puntos de la duración de una solicitud.

Controladores de eventos para el `HttpApplication` eventos pueden colocarse en un archivo especial denominado `Global.asax`. Para crear este archivo en su sitio Web, agregar un nuevo elemento a la raíz del sitio Web utilizando la plantilla de clase de aplicación Global con el nombre `Global.asax`.

[![](processing-unhandled-exceptions-cs/_static/image2.png)](processing-unhandled-exceptions-cs/_static/image1.png)

**Figura 1**: agregar `Global.asax` a la aplicación Web  
([Haga clic aquí para ver la imagen a tamaño completo](processing-unhandled-exceptions-cs/_static/image3.png))

El contenido y la estructura de la `Global.asax` archivo creado por Visual Studio varían ligeramente en función de si usas un proyecto de aplicación Web (WAP) o un proyecto de sitio Web (WSP). Con un WAP, el `Global.asax` se implementa como dos archivos independientes - `Global.asax` y `Global.asax.cs`. El `Global.asax` archivo no contiene nada, pero un `@Application` directiva que hace referencia el `.cs` archivo; el evento se definen los controladores de interés en el `Global.asax.cs` archivo. Para WSPs, se crea un único archivo, `Global.asax`, y los controladores de eventos se definen en un `<script runat="server">` bloque.

El `Global.asax` archivo creado en un WAP por plantilla de clase de aplicación Global de Visual Studio incluye controladores de eventos denominados `Application_BeginRequest`, `Application_AuthenticateRequest`, y `Application_Error`, que son controladores de eventos para el `HttpApplication` eventos `BeginRequest`, `AuthenticateRequest`, y `Error`, respectivamente. También hay controladores de eventos denominados `Application_Start`, `Session_Start`, `Application_End`, y `Session_End`, que son controladores de eventos que se desencadenan cuando la aplicación web se inicia, cuando se inicia una nueva sesión, cuando finaliza la aplicación y, cuando finaliza una sesión, respectivamente. El `Global.asax` archivo creado en un WSP por Visual Studio contiene solo el `Application_Error`, `Application_Start`, `Session_Start`, `Application_End`, y `Session_End` controladores de eventos.

> [!NOTE]
> Al implementar la aplicación ASP.NET debe copiar el `Global.asax` archivo al entorno de producción. El `Global.asax.cs` no es necesario el archivo, que se crea en el WAP, que se copiará en producción porque este código se compila en el ensamblado del proyecto.


Los controladores de eventos creados por la plantilla de clase de aplicación Global de Visual Studio no son exhaustivos. Puede agregar un controlador de eventos para cualquier `HttpApplication` evento por el controlador de eventos de nomenclatura `Application_EventName`. Por ejemplo, podría agregar el código siguiente a la `Global.asax` archivo para crear un controlador de eventos para el [ `AuthorizeRequest` eventos](https://msdn.microsoft.com/library/system.web.httpapplication.authorizerequest.aspx):

[!code-cs[Main](processing-unhandled-exceptions-cs/samples/sample1.cs)]

Igualmente, puede quitar los controladores de eventos creados por la plantilla de clase de aplicación Global que no son necesarios. En este tutorial sólo es necesario que un controlador de eventos para el `Error` eventos; Llamarme quitar los otros controladores de eventos desde el `Global.asax` archivo.

> [!NOTE]
> *Módulos HTTP* ofrecen otra forma de definir controladores de eventos para `HttpApplication` eventos. Los módulos HTTP se crean como un archivo de clase que se pueden colocar directamente en el proyecto de aplicación web o separarse en una biblioteca de clases independiente. Debido a que se pueden separar en una biblioteca de clases, módulos HTTP ofrecen un modelo más flexible y reutilizable para crear `HttpApplication` controladores de eventos. Mientras que la `Global.asax` archivo es específico a la aplicación web que se encuentra, se pueden compilar módulos HTTP en ensamblados, momento en que agregar el módulo HTTP a un sitio Web es tan sencillo como colocando el ensamblado el `Bin` carpeta y registrar el Módulo en `Web.config`. Este tutorial no se parece al crear y utilizar módulos HTTP, pero las bibliotecas de registro de errores en dos utilizadas en los siguientes dos tutoriales se implementan como módulos HTTP. Para obtener más información sobre las ventajas de los módulos HTTP, consulte [mediante módulos de HTTP y los controladores al crear componentes de ASP.NET conectables](https://msdn.microsoft.com/library/aa479332.aspx).


## <a name="retrieving-information-about-the-unhandled-exception"></a>Al recuperar la información sobre la excepción no controlada

En este momento tenemos un archivo Global.asax con un `Application_Error` controlador de eventos. Cuando se ejecuta este controlador de eventos es necesario notificar a un desarrollador del error y los detalles de registro. Para realizar estas tareas, primero necesitamos determinar los detalles de la excepción que se ha producido. Usar el objeto de servidor [ `GetLastError` método](https://msdn.microsoft.com/library/system.web.httpserverutility.getlasterror.aspx) recuperar detalles de la excepción no controlada que produjo la `Error` activación del evento.

[!code-csharp[Main](processing-unhandled-exceptions-cs/samples/sample2.cs)]

El `GetLastError` método devuelve un objeto de tipo `Exception`, que es el tipo base para todas las excepciones en .NET Framework. Sin embargo, en el código anterior estoy convertir el objeto de excepción devolviendo por `GetLastError` en un `HttpException` objeto. Si el `Error` evento se desencadena porque se produjo una excepción durante el procesamiento de un recurso ASP.NET, a continuación, se ajusta dentro de la excepción que se produjo una `HttpException`. Para obtener la excepción real que motivó el uso de eventos de Error del `InnerException` propiedad. Si el `Error` se generó el evento debido a una excepción basada en HTTP, como una solicitud para una página inexistente, un `HttpException` se produce, pero no tiene una excepción interna.

El siguiente código utiliza el `GetLastErrormessage` para recuperar información sobre la excepción que desencadenó la `Error` evento, almacenar el `HttpException` en una variable denominada `lastErrorWrapper`. A continuación, almacena el tipo, el mensaje y el seguimiento de la pila de la excepción original en tres variables de cadena, comprueba si el `lastErrorWrapper` es la excepción real que desencadenó la `Error` evento (en el caso de excepciones basado en HTTP) o si es simplemente un contenedor para una excepción que se produjo al procesar la solicitud.

[!code-csharp[Main](processing-unhandled-exceptions-cs/samples/sample3.cs)]

En este momento tiene toda la información que necesita escribir código que se registrará los detalles de la excepción en una tabla de base de datos. Puede crear una tabla de base de datos con columnas para cada uno de los detalles del error de interés - el tipo, el mensaje, el seguimiento de la pila y así sucesivamente - junto con otras piezas útiles de información, como la dirección URL de la página solicitada y el nombre del usuario que ha iniciado sesión actualmente. En el `Application_Error` controlador de eventos, a continuación, podría conectarse a la base de datos e insertar un registro en la tabla. Del mismo modo, puede agregar código para un programador del error a través de correo electrónico de alerta.

Las bibliotecas de registro de error examinadas en los siguientes dos tutoriales proporcionan esta funcionalidad de fábrica, por lo que no es necesario para compilar este registro de errores y la notificación. Sin embargo, para ilustrar que el `Error` se ha generado el evento y que la `Application_Error` controlador de eventos puede utilizarse para registrar los detalles del error y notificar a un programador, vamos a agregar código que notifica a un desarrollador cuando se produce un error.

## <a name="notifying-a-developer-when-an-unhandled-exception-occurs"></a>Notificar a un programador cuando se produce una excepción no controlada

Cuando se produce una excepción no controlada en el entorno de producción es importante avisar al equipo de desarrollo para que pueda evaluar el error y determinar qué acciones deben realizarse. Por ejemplo, si se produce un error en la conexión a la base de datos, deberá en tipo doble Compruebe la cadena de conexión y, quizás, abra una incidencia de soporte técnico con la empresa de hospedaje web. Si la excepción se produjo debido a un error de programación, código adicional ni una lógica de validación que necesite agregar para evitar estos errores en el futuro.

Clases de .NET Framework en el [ `System.Net.Mail` espacio de nombres](https://msdn.microsoft.com/library/system.net.mail.aspx) resulta fácil enviar un correo electrónico. El [ `MailMessage` clase](https://msdn.microsoft.com/library/system.net.mail.mailmessage.aspx) representa un mensaje de correo electrónico y tiene propiedades como `To`, `From`, `Subject`, `Body`, y `Attachments`. El `SmtpClass` se usa para enviar un `MailMessage` el objeto con un servidor SMTP especificado; se puede especificar la configuración del servidor SMTP mediante programación o mediante declaración en el [ `<system.net>` elemento](https://msdn.microsoft.com/library/6484zdc1.aspx) en el `Web.config file`. Para obtener más información sobre el envío de correo electrónico mensajes de una aplicación ASP.NET Vea mi artículo [enviar correo electrónico en ASP.NET](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx)y el [System.Net.Mail preguntas más frecuentes sobre](http://systemnetmail.com/).

> [!NOTE]
> El `<system.net>` elemento contiene la configuración del servidor SMTP utilizada por la `SmtpClient` clase al enviar un correo electrónico. Su empresa probable de hospedaje tiene un servidor SMTP que puede usar para enviar correo electrónico desde la aplicación. Consulte la sección de soporte técnico de su host de web para obtener información sobre la configuración del servidor SMTP que debe usar en la aplicación web.


Agregue el código siguiente a la `Application_Error` controlador de eventos para enviar un correo electrónico de un desarrollador cuando se produce un error:

[!code-csharp[Main](processing-unhandled-exceptions-cs/samples/sample4.cs)]

Mientras que el código anterior es muy largo, la mayor parte del mismo crea el código HTML que aparece en el correo electrónico enviado al desarrollador de la. El código que se inicia haciendo referencia a la `HttpException` devuelto por la `GetLastError` (método) (`lastErrorWrapper`). La excepción que se produjo por la solicitud se recupera a través de `lastErrorWrapper.InnerException` y se asigna a la variable `lastError`. El tipo, el mensaje y la pila se recupera información de seguimiento de `lastError` y se almacenan en tres variables de cadena.

Después, un `MailMessage` objeto denominado `mm` se crea. El cuerpo del correo electrónico es HTML con formato y muestra la dirección URL de la página solicitada, el nombre del usuario con sesión iniciada actualmente y obtener información sobre la excepción (tipo, mensaje y seguimiento de la pila). Una de las cosas interesantes sobre la `HttpException` clase es que puede generar el código HTML utilizado para crear la excepción detalles amarillo pantalla de muerte (YSOD) mediante una llamada a la [GetHtmlErrorMessage método](https://msdn.microsoft.com/library/system.web.httpexception.gethtmlerrormessage.aspx). Este método se utiliza a continuación para recuperar el marcado YSOD de detalles de excepción y agregarlo al correo electrónico como datos adjuntos. Una palabra de precaución: si la excepción que desencadenó la `Error` evento se produjo una excepción basada en HTTP (por ejemplo, una solicitud para una página inexistente) la `GetHtmlErrorMessage` método devolverá `null`.

El paso final consiste en enviar el `MailMessage`. Esto se hace mediante la creación de un nuevo `SmtpClient` método y llamar a su `Send` método.

> [!NOTE]
> Antes de usar este código en la aplicación web desea cambiar los valores de la `ToAddress` y `FromAddress` constantes de support@example.com al correo electrónico de cualquier dirección de correo electrónico de notificación de error, se debe enviar y proceden de. También necesitará especificar la configuración del servidor SMTP en el `<system.net>` sección `Web.config`. Consulte a su proveedor de host de web para determinar la configuración del servidor SMTP para usar.


Con este código en su lugar siempre que hay un error el programador se envía un mensaje de correo electrónico que resume el error e incluye el YSOD. En el tutorial anterior se muestra un error en tiempo de ejecución, visite Genre.aspx y se pasa una no es válido `ID` valor a través de la cadena de consulta, como `Genre.aspx?ID=foo`. Visite la página con el `Global.asax` archivo en su lugar genera la misma experiencia de usuario, como en el tutorial anterior - en el entorno de desarrollo seguirá ver la excepción detalles amarillo pantalla de muerte, mientras que en el entorno de producción Consulte la página de error personalizado. Además de este comportamiento existente, el programador se envía un correo electrónico.

**Figura 2** muestra el correo electrónico recibido al visitar `Genre.aspx?ID=foo`. El cuerpo del correo electrónico resume la información de excepción, mientras que la `YSOD.htm` datos adjuntos muestra el contenido que se muestra en el YSOD de detalles de excepción (consulte **figura 3**).

[![](processing-unhandled-exceptions-cs/_static/image5.png)](processing-unhandled-exceptions-cs/_static/image4.png)

**Figura 2**: el programador se envía una notificación por correo electrónico cada vez que hay una excepción no controlada  
([Haga clic aquí para ver la imagen a tamaño completo](processing-unhandled-exceptions-cs/_static/image6.png))

[![](processing-unhandled-exceptions-cs/_static/image8.png)](processing-unhandled-exceptions-cs/_static/image7.png)

**Figura 3**: la notificación de correo electrónico incluye los detalles de excepción YSOD como datos adjuntos  
([Haga clic aquí para ver la imagen a tamaño completo](processing-unhandled-exceptions-cs/_static/image9.png))

## <a name="what-about-using-the-custom-error-page"></a>¿Qué ocurre con la página de Error personalizado?

Este tutorial ha mostrado cómo usar `Global.asax` y `Application_Error` controlador de eventos para ejecutar código cuando se produce una excepción no controlada. En concreto, se utiliza este controlador de eventos para notificar a un desarrollador de un error; podríamos ampliarlo para registrar también los detalles del error en una base de datos. La presencia de la `Application_Error` controlador de eventos no afecta a la experiencia del usuario final. Todavía verán la página de error configurado, ya sea la YSOD detalles del Error, el YSOD de Error en tiempo de ejecución o la página de error personalizado.

Es natural que se pregunte si el `Global.asax` archivo y `Application_Error` eventos es necesario cuando se utiliza una página de error personalizada. Cuando se produce un error, el usuario se muestra la página de error personalizada por lo tanto ¿por qué no se coloque el código para notificar al desarrollador y registrar los detalles del error en la clase de código subyacente de la página de error personalizada? Mientras que por supuesto, puede agregar código a la clase de código subyacente de la página de error personalizado no tiene acceso a los detalles de la excepción que desencadenó la `Error` eventos cuando se utiliza la técnica que se explora en el tutorial anterior. Llamar a la `GetLastError` método desde la página de error personalizado devuelve `Nothing`.

El motivo de este comportamiento es porque se alcanza la página de error personalizada a través de una redirección. Cuando una excepción no controlada alcanza el tiempo de ejecución ASP.NET que el motor ASP.NET genera su `Error` evento (que se ejecuta el `Application_Error` controlador de eventos) y, a continuación, *redirige* al usuario a la página de error personalizado mediante la emisión de un `Response.Redirect(customErrorPageUrl)`. El `Response.Redirect` método envía una respuesta al cliente con un código de estado HTTP 302, que indica el explorador para solicitar una nueva dirección URL, es decir, la página de error personalizado. A continuación, el explorador solicita automáticamente esta nueva página. Puede indicar que la página de error personalizada solicitada por separado desde la página donde se originó el error porque la barra de direcciones del explorador cambia a la URL de la página de error personalizado (vea **figura 4**).

[![](processing-unhandled-exceptions-cs/_static/image11.png)](processing-unhandled-exceptions-cs/_static/image10.png)

**Figura 4**: cuando se produce un Error, el explorador se redirige a la URL de la página de Error personalizado  
([Haga clic aquí para ver la imagen a tamaño completo](processing-unhandled-exceptions-cs/_static/image12.png))

El efecto neto es que la solicitud que se produjo la excepción no controlada finaliza cuando el servidor responde con la redirección HTTP 302. La solicitud posterior a la página de error personalizada es una solicitud nueva; Llegados a este punto ASP.NET motor ha descartado la información de error y, además, no tiene ninguna manera para asociar la excepción no controlada en la solicitud anterior a la nueva solicitud de la página de error personalizada. Se trata de por qué `GetLastError` devuelve `null` cuando se llama desde la página de error personalizado.

Sin embargo, es posible que la página de error personalizada que se ejecutan durante la misma solicitud que produjo el error. El [ `Server.Transfer(url)` ](https://msdn.microsoft.com/library/system.web.httpserverutility.transfer.aspx) método transfiere la ejecución a la dirección URL especificada y lo procesa en la misma solicitud. Puede cambiar el código el `Application_Error` controlador de eventos para la clase de código subyacente de la página de error personalizada, reemplazarlo en `Global.asax` con el código siguiente:

[!code-csharp[Main](processing-unhandled-exceptions-cs/samples/sample5.cs)]

Ahora cuando se produce una excepción no controlada del `Application_Error` controlador de eventos transfiere el control a la página de error personalizado apropiado en función del código de estado HTTP. Dado que el control se transfiere, la página de error personalizada tiene acceso a la información de excepción no controlada mediante `Server.GetLastError` y puede notificar a un desarrollador del error y los detalles de registro. El `Server.Transfer` llamada detiene el motor ASP.NET de redirigir al usuario a la página de error personalizado. En su lugar, se devuelve el contenido de la página de error personalizada como respuesta a la página que generó el error.

## <a name="summary"></a>Resumen

Cuando se produce una excepción no controlada en una aplicación web ASP.NET en tiempo de ejecución ASP.NET genera el `Error` eventos y muestra la página de error configurado. Podemos notificar el desarrollador del error, los detalles de registro o procesar de otra forma, mediante la creación de un controlador de eventos para el evento de Error. Hay dos maneras de crear un controlador de eventos para `HttpApplication` eventos como `Error`: en el `Global.asax` archivo o desde un módulo HTTP. Este tutorial ha mostrado cómo crear un `Error` controlador de eventos en el `Global.asax` archivo que notifica a los desarrolladores de un error por medio de un mensaje de correo electrónico.

Crear un `Error` es útil si tiene que procesar las excepciones no controladas de alguna manera personalizada o un único controlador de eventos. Sin embargo, crear su propio `Error` controlador de eventos para registrar la excepción o para notificar a un desarrollador no es el uso más eficaz del tiempo como ya existen las bibliotecas de registro de error gratuito y fácil de usar que se puede configurar en cuestión de minutos. Los siguientes dos tutoriales examinan dos de estas bibliotecas.

Feliz programación.

### <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas tratados en este tutorial, consulte los siguientes recursos:

- [Información general de los controladores HTTP y módulos HTTP de ASP.NET](https://support.microsoft.com/kb/307985)
- [Responde correctamente a las excepciones no controladas - procesar las excepciones no controladas](http://aspnet.4guysfromrolla.com/articles/091306-1.aspx)
- [`HttpApplication` Clase y el objeto de aplicación de ASP.NET](http://www.eggheadcafe.com/articles/20030211.asp)
- [Controladores HTTP y módulos HTTP de ASP.NET](http://www.15seconds.com/Issue/020417.htm)
- [Enviar correo electrónico en ASP.NET](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx)
- [Descripción de la `Global.asax` archivo](http://aspalliance.com/1114_Understanding_the_Globalasax_file.all)
- [Usar módulos HTTP y controladores para crear componentes ASP.NET conectables](https://msdn.microsoft.com/library/aa479332.aspx)
- [Trabajar con ASP.NET `Global.asax` archivo](http://articles.techrepublic.com.com/5100-10878_11-5771721.html)
- [Trabajar con `HttpApplication` instancias](https://msdn.microsoft.com/library/a0xez8f2.aspx)

> [!div class="step-by-step"]
> [Anterior](displaying-a-custom-error-page-cs.md)
> [Siguiente](logging-error-details-with-asp-net-health-monitoring-cs.md)
