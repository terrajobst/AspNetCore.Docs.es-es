---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/processing-unhandled-exceptions-vb
title: Procesar excepciones no controladas (VB) | Microsoft Docs
author: rick-anderson
description: Cuando se produce un error en tiempo de ejecución en una aplicación web en producción es importante para notificar a un desarrollador y que registre el error, por lo que es posible que se diagnostican en a la...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/09/2009
ms.topic: article
ms.assetid: 051296f0-9519-4e78-835c-d868da13b0a0
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/processing-unhandled-exceptions-vb
msc.type: authoredcontent
ms.openlocfilehash: 6c0f6883f2a19d060aff1573238cc1df1838e824
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37365989"
---
<a name="processing-unhandled-exceptions-vb"></a>Procesar excepciones no controladas (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnet/web-forms/overview/older-versions-getting-started/deploying-web-site-projects/processing-unhandled-exceptions-vb/samples) ([cómo descargarlo](/aspnet/core/tutorials/index#how-to-download-a-sample))

> Cuando se produce un error en tiempo de ejecución en una aplicación web en producción es importante para notificar a un desarrollador y que registre el error para que se pueden diagnosticar en un momento posterior en el tiempo. En este tutorial se proporciona información general sobre cómo ASP.NET procesa los errores en tiempo de ejecución y examina una manera de tener código personalizado se ejecute cada vez que un burbujas de excepción no controlada al tiempo de ejecución de ASP.NET.


## <a name="introduction"></a>Introducción

Cuando se produce una excepción no controlada en una aplicación ASP.NET, propaga hasta el tiempo de ejecución ASP.NET, lo que provoca la `Error` eventos y muestra la página de error adecuado. Hay tres tipos diferentes de las páginas de error: el tiempo de ejecución Error amarillo pantalla de muerte (YSOD); los detalles de excepción YSOD; y páginas de errores personalizados. En el [tutorial anterior](displaying-a-custom-error-page-vb.md) hemos configurado la aplicación para usar una página de error personalizada para usuarios remotos y la YSOD de detalles de excepción para los usuarios que visitan localmente.

Uso de una página de error personalizada fácil de usar que coincida con la apariencia y funcionamiento del sitio es preferido para el valor predeterminado YSOD de Error en tiempo de ejecución, pero mostrar una página de error personalizada es sólo una parte de una completa solución de control de errores. Cuando se produce un error en una aplicación en producción, es importante que los desarrolladores se notifican el error para que puedan saque la causa de la excepción a la luz y solucionarlo. Además, se deben registrar los detalles del error para que el error puede examinarse y diagnostica en un momento posterior en el tiempo.

Este tutorial muestra cómo obtener acceso a los detalles de una excepción no controlada para que se inician y desarrollador de una notificación. Los dos tutoriales sigue este uno exploran las bibliotecas de registro de error que, después de un poco de configuración, notificar a los desarrolladores de errores en tiempo de ejecución y sus detalles de registro automáticamente.

> [!NOTE]
> La información que se examinan en este tutorial es muy útil si necesita procesar excepciones no controladas de alguna manera único o personalizada. En casos donde sólo necesita registrar la excepción y notificar a un desarrollador, mediante una biblioteca de registro de error es la mejor opción. Los siguientes dos tutoriales proporcionan una visión general de estas dos bibliotecas.


## <a name="executing-code-when-theerrorevent-is-raised"></a>Ejecutar código cuando el`Error`provoca el evento

Los eventos proporcionan un mecanismo de señalización que ha sucedido algo interesante y de otro objeto para ejecutar código en respuesta de un objeto. Como desarrollador de ASP.NET está acostumbrado a pensar en términos de eventos. Si desea ejecutar código cuando el visitante hace clic en un botón determinado, cree un controlador de eventos para ese botón `Click` evento y poner el código allí. Dado que el tiempo de ejecución ASP.NET genera su [ `Error` eventos](https://msdn.microsoft.com/library/system.web.httpapplication.error.aspx) cada vez que se produce una excepción no controlada, se supone que el código para registrar los detalles del error iría en un controlador de eventos. Pero ¿cómo se crea un controlador de eventos para el `Error` eventos?

El `Error` evento es uno de muchos eventos en el [ `HttpApplication` clase](https://msdn.microsoft.com/library/system.web.httpapplication.aspx) que se generan en ciertas fases de la canalización HTTP durante la duración de una solicitud. Por ejemplo, el `HttpApplication` la clase [ `BeginRequest` eventos](https://msdn.microsoft.com/library/system.web.httpapplication.beginrequest.aspx) se produce al principio de cada solicitud; su [ `AuthenticateRequest` evento](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx) se genera cuando un módulo de seguridad ha identificado el solicitante. Estos `HttpApplication` eventos ofrecen al desarrollador de páginas de un medio para ejecutar la lógica personalizada en los diferentes puntos de la duración de una solicitud.

Controladores de eventos para el `HttpApplication` eventos se pueden colocar en un archivo especial denominado `Global.asax`. Para crear este archivo en su sitio Web, agregue un nuevo elemento a la raíz de su sitio Web mediante la plantilla de clase de aplicación Global con el nombre `Global.asax`.

[![](processing-unhandled-exceptions-vb/_static/image2.png)](processing-unhandled-exceptions-vb/_static/image1.png)

**Figura 1**: agregar `Global.asax` a la aplicación Web  
 ([Haga clic aquí para ver imagen en tamaño completo](processing-unhandled-exceptions-vb/_static/image3.png))

El contenido y la estructura de la `Global.asax` archivo creado por Visual Studio varían ligeramente en función de si usas un proyecto de aplicación Web (WAP) o un proyecto de sitio Web (WSP). Con un protocolo WAP, el `Global.asax` se implementa como dos archivos distintos - `Global.asax` y `Global.asax.vb`. El `Global.asax` archivo no contiene nada, pero un `@Application` directiva que hace referencia el `.vb` archivo; el evento se definen los controladores de interés en el `Global.asax.vb` archivo. Para wsp, se crea un solo archivo, `Global.asax`, y los controladores de eventos se definen en un `<script runat="server">` bloque.

El `Global.asax` archivo creado en un WAP mediante la plantilla de clase de aplicación Global de Visual Studio incluye los controladores de eventos denominados `Application_BeginRequest`, `Application_AuthenticateRequest`, y `Application_Error`, que son controladores de eventos para el `HttpApplication` eventos `BeginRequest`, `AuthenticateRequest`, y `Error`, respectivamente. También hay controladores de eventos denominados `Application_Start`, `Session_Start`, `Application_End`, y `Session_End`, que son controladores de eventos que se activan cuando la aplicación web se inicia, cuando se inicia una nueva sesión, cuando finaliza la aplicación y, cuando finaliza una sesión, respectivamente. El `Global.asax` archivo creado en un archivo WSP por Visual Studio contiene sólo el `Application_Error`, `Application_Start`, `Session_Start`, `Application_End`, y `Session_End` controladores de eventos.

> [!NOTE]
> Al implementar la aplicación de ASP.NET debe copiar el `Global.asax` archivo en el entorno de producción. El `Global.asax.vb` no es necesario el archivo, que se crea en el WAP, que se copiarán en producción porque este código se compila en el ensamblado del proyecto.


Los controladores de eventos creados por la plantilla de clase de aplicación Global de Visual Studio no son exhaustivas. Puede agregar un controlador de eventos para cualquier `HttpApplication` eventos asignando el controlador de eventos `Application_EventName`. Por ejemplo, podría agregar el código siguiente a la `Global.asax` archivo para crear un controlador de eventos para el [ `AuthorizeRequest` eventos](https://msdn.microsoft.com/library/system.web.httpapplication.authorizerequest.aspx):

[!code-vb[Main](processing-unhandled-exceptions-vb/samples/sample1.vb)]

Del mismo modo, puede quitar los controladores de eventos creados por la plantilla de clase de aplicación Global que no son necesarios. En este tutorial solo se requiere un controlador de eventos para el `Error` eventos; si quiere, puede quitar los otros controladores de eventos desde el `Global.asax` archivo.

> [!NOTE]
> *Los módulos HTTP* ofrecen otra manera de definir controladores de eventos para `HttpApplication` eventos. Los módulos HTTP se crean como un archivo de clase que se puede colocar directamente en el proyecto de aplicación web o separaron en una biblioteca de clases independiente. Debido a pueden dividirse en una biblioteca de clases, módulos HTTP ofrecen un modelo más flexible y reutilizable para crear `HttpApplication` controladores de eventos. Mientras que la `Global.asax` archivo es específico para la aplicación web en el que reside, los módulos HTTP se pueden compilar en ensamblados, momento en que agregar el módulo HTTP a un sitio Web es tan sencillo como colocando el ensamblado el `Bin` carpeta y registrar el Módulo de `Web.config`. En este tutorial no se fija en la creación y uso de módulos HTTP, pero las bibliotecas de registro de errores en dos utilizadas en los siguientes tutoriales de dos se implementan como módulos HTTP. Para obtener más información sobre las ventajas de los módulos HTTP que hacen referencia a [uso de módulos y controladores HTTP para crear los componentes ASP.NET conectables](https://msdn.microsoft.com/library/aa479332.aspx).


## <a name="retrieving-information-about-the-unhandled-exception"></a>Al recuperar la información sobre la excepción no controlada

En este momento, tenemos un archivo Global.asax con un `Application_Error` controlador de eventos. Cuando se ejecuta este controlador de eventos, necesitamos notificar a un desarrollador del error y sus detalles de registro. Para realizar estas tareas, primero necesitamos determinar los detalles de la excepción que se generó. Usar el objeto de servidor [ `GetLastError` método](https://msdn.microsoft.com/library/system.web.httpserverutility.getlasterror.aspx) para recuperar los detalles de la excepción no controlada que provocó la `Error` activación del evento.

[!code-vb[Main](processing-unhandled-exceptions-vb/samples/sample2.vb)]

El `GetLastError` método devuelve un objeto de tipo `Exception`, que es el tipo base para todas las excepciones en .NET Framework. Sin embargo, en el código anterior estoy conversión devuelto por el objeto de excepción `GetLastError` en un `HttpException` objeto. Si el `Error` evento se desencadena porque se produjo una excepción durante el procesamiento de un recurso de ASP.NET, a continuación, se ajusta la excepción que se produjo dentro de un `HttpException`. Para obtener la excepción real que originó el uso de eventos de Error del `InnerException` propiedad. Si el `Error` se generó el evento debido a una excepción basada en HTTP, como una solicitud para una página inexistente, un `HttpException` se inicia, pero no tiene una excepción interna.

El siguiente código utiliza el `GetLastErrormessage` para recuperar información sobre la excepción que desencadenó la `Error` evento, almacenar el `HttpException` en una variable denominada `lastErrorWrapper`. A continuación, almacena el tipo, el mensaje y el seguimiento de la pila de la excepción original en tres variables de cadena, comprueba si el `lastErrorWrapper` es la excepción real que desencadenó la `Error` evento (en el caso de excepciones basado en HTTP) o si se trata simplemente una contenedor para una excepción que se produjo al procesar la solicitud.

[!code-vb[Main](processing-unhandled-exceptions-vb/samples/sample3.vb)]

En este momento tiene toda la información que necesita escribir código que se registrará los detalles de la excepción en una tabla de base de datos. Podría crear una tabla de base de datos con columnas para cada uno de los detalles del error de interés: el tipo, el mensaje, el seguimiento de la pila y así sucesivamente - junto con otras piezas útiles de información, como la dirección URL de la página solicitada y el nombre del usuario que ha iniciado sesión actualmente. En el `Application_Error` controlador de eventos, a continuación, podría conectarse a la base de datos e insertar un registro en la tabla. Del mismo modo, puede agregar código para un desarrollador del error a través de correo electrónico de alerta.

Las bibliotecas de registro de error examinadas en los próximos dos tutoriales proporcionan esta funcionalidad desde el principio, por lo que no es necesario para compilar este registro de errores y la notificación. Sin embargo, para ilustrar que el `Error` es que se provoca el evento y que el `Application_Error` controlador de eventos puede usarse para registrar los detalles del error y notificar a un desarrollador, vamos a agregar código que notifica a un desarrollador cuando se produce un error.

## <a name="notifying-a-developer-when-an-unhandled-exception-occurs"></a>Notificar a un desarrollador cuando se produce una excepción no controlada

Cuando se produce una excepción no controlada en el entorno de producción es importante avisar al equipo de desarrollo para que pueda evaluar el error y determinar qué acciones deben realizarse. Por ejemplo, si se produce un error al conectar a la base de datos, a continuación, tendrá que double Compruebe la cadena de conexión y, quizás, abra una incidencia de soporte técnico con su empresa de hospedaje web. Si se produjo la excepción debido a un error de programación, código adicional ni lógica de validación deba agregarse a evitar tales errores en el futuro.

Las clases de .NET Framework en el [ `System.Net.Mail` espacio de nombres](https://msdn.microsoft.com/library/system.net.mail.aspx) facilitan la tarea Enviar un correo electrónico. El [ `MailMessage` clase](https://msdn.microsoft.com/library/system.net.mail.mailmessage.aspx) representa un mensaje de correo electrónico y tiene propiedades como `To`, `From`, `Subject`, `Body`, y `Attachments`. El `SmtpClass` se usa para enviar un `MailMessage` objeto utilizando un servidor SMTP especificado; se puede especificar la configuración del servidor SMTP mediante programación o mediante declaración en el [ `<system.net>` elemento](https://msdn.microsoft.com/library/6484zdc1.aspx) en el `Web.config file`. Para obtener más información sobre el envío de correo electrónico de los mensajes en una aplicación ASP.NET consulte mi artículo, [enviar correo electrónico en ASP.NET](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx)y el [System.Net.Mail preguntas más frecuentes sobre](http://systemnetmail.com/).

> [!NOTE]
> El `<system.net>` elemento contiene la configuración del servidor SMTP utilizada por el `SmtpClient` clase al enviar un correo electrónico. Su empresa probable de hospedaje tiene un servidor SMTP que puede usar para enviar correo electrónico desde su aplicación. Consulte la sección de soporte técnico de su host de web para obtener información sobre la configuración del servidor SMTP que debe usar en su aplicación web.


Agregue el código siguiente a la `Application_Error` controlador de eventos para enviar un correo electrónico de un desarrollador cuando se produce un error:

[!code-vb[Main](processing-unhandled-exceptions-vb/samples/sample4.vb)]

Aunque el código anterior es bastante largo, la mayor parte de él crea el HTML que aparece en el correo electrónico enviado a los desarrolladores. El código comienza por hacer referencia a la `HttpException` devuelto por la `GetLastError` método (`lastErrorWrapper`). La excepción que se generó la solicitud se recupera mediante `lastErrorWrapper.InnerException` y se asigna a la variable `lastError`. El tipo, el mensaje y la pila se recupera información de seguimiento de `lastError` y se almacenan en tres variables de cadena.

A continuación, un `MailMessage` objeto denominado `mm` se crea. El cuerpo del correo electrónico es HTML con formato y muestra la dirección URL de la página solicitada, el nombre del usuario ha iniciado sesión actualmente y obtener información sobre la excepción (tipo, mensaje y seguimiento de la pila). Una de las cosas interesantes sobre la `HttpException` clase es que puede generar el código HTML utilizado para crear la excepción detalles amarillo pantalla de muerte (YSOD) mediante una llamada a la [GetHtmlErrorMessage método](https://msdn.microsoft.com/library/system.web.httpexception.gethtmlerrormessage.aspx). Este método se usa aquí para recuperar el marcado YSOD detalles de excepción y agregarlo al correo electrónico como datos adjuntos. Una palabra de advertencia: si la excepción que desencadena la `Error` eventos produjo una excepción basada en HTTP (por ejemplo, una solicitud para una página inexistente) el `GetHtmlErrorMessage` método devolverá `null`.

El último paso consiste en enviar la `MailMessage`. Esto se hace creando un nuevo `SmtpClient` método y llamar a su `Send` método.

> [!NOTE]
> Antes de usar este código en su aplicación web desea cambiar los valores de la `ToAddress` y `FromAddress` constantes de support@example.com a cualquier correo electrónico se enviarán a dirección de correo electrónico de notificación de error y se originan en. También deberá especificar la configuración del servidor SMTP en el `<system.net>` sección `Web.config`. Consulte a su proveedor de host de web para determinar la configuración del servidor SMTP para usar.


Con este código en su lugar siempre que hay un error al desarrollador se envía un mensaje de correo electrónico que resume el error e incluye el YSOD. En el tutorial anterior se mostró un error en tiempo de ejecución al visitar Genre.aspx y pasando un no válido `ID` valor a través de la cadena de consulta, como `Genre.aspx?ID=foo`. Visitar la página con el `Global.asax` archivo en su lugar genera la misma experiencia de usuario como en el tutorial anterior: en el entorno de desarrollo seguirá apareciendo el excepción detalles amarillo pantalla de la muerte, mientras que en el entorno de producción deberá Consulte la página de error personalizado. Además de este comportamiento existente, el programador se envía un correo electrónico.

**Figura 2** muestra el correo electrónico recibido cuando se visita `Genre.aspx?ID=foo`. El cuerpo del correo electrónico resume la información de excepción, mientras que el `YSOD.htm` adjunto muestra el contenido que se muestra en el YSOD de detalles de excepción (vea **figura 3**).

[![](processing-unhandled-exceptions-vb/_static/image5.png)](processing-unhandled-exceptions-vb/_static/image4.png)

**Figura 2**: el desarrollador se envía una notificación por correo electrónico siempre que se produce una excepción no controlada  
 ([Haga clic aquí para ver imagen en tamaño completo](processing-unhandled-exceptions-vb/_static/image6.png))

[![](processing-unhandled-exceptions-vb/_static/image8.png)](processing-unhandled-exceptions-vb/_static/image7.png)

**Figura 3**: la notificación de correo electrónico incluye los detalles de excepción YSOD como datos adjuntos  
 ([Haga clic aquí para ver imagen en tamaño completo](processing-unhandled-exceptions-vb/_static/image9.png))

## <a name="what-about-using-the-custom-error-page"></a>¿Qué sucede con la página de Error personalizado?

Este tutorial ha mostrado cómo usar `Global.asax` y `Application_Error` controlador de eventos para ejecutar código cuando se produce una excepción no controlada. En concreto, se utiliza este controlador de eventos para notificar a un desarrollador de un error; Podríamos extender para también registrar los detalles del error en una base de datos. La presencia de la `Application_Error` controlador de eventos no afecta a la experiencia del usuario final. Aún verán la página de error configurado, ya sea la YSOD detalles del Error, el YSOD de Error en tiempo de ejecución o la página de error personalizado.

Es natural preguntarse si el `Global.asax` archivo y `Application_Error` eventos es necesario cuando se usa una página de error personalizada. Cuando se produce un error, el usuario se muestra la página de error personalizado así que ¿por qué no podemos colocamos el código para notificar a los desarrolladores y los detalles del error de registro en la clase de código subyacente de la página de error personalizado? Aunque sin duda, puede agregar código a la clase de código subyacente de la página de error personalizado no tiene acceso a los detalles de la excepción que desencadenó la `Error` eventos cuando se usa la técnica que se explora en el tutorial anterior. Una llamada a la `GetLastError` devuelve el método de la página de error personalizada `Nothing`.

La razón de este comportamiento es que llegar a través de un redireccionamiento a la página de error personalizado. Cuando una excepción no controlada alcanza el tiempo de ejecución ASP.NET que el motor ASP.NET genera su `Error` evento (que se ejecuta el `Application_Error` controlador de eventos) y, a continuación, *redirige* al usuario a la página de error personalizado mediante la emisión de un `Response.Redirect(customErrorPageUrl)`. El `Response.Redirect` método envía una respuesta al cliente con un código de estado HTTP 302, indicando el explorador para solicitar una nueva dirección URL, es decir, la página de error personalizado. A continuación, el explorador solicita automáticamente esta nueva página. Puede indicar que la página de error personalizado solicitada por separado desde la página donde se originó el error porque la barra de direcciones del explorador cambia a la dirección URL de página de error personalizado (vea **figura 4**).

[![](processing-unhandled-exceptions-vb/_static/image11.png)](processing-unhandled-exceptions-vb/_static/image10.png)

**Figura 4**: cuando se produce un Error, el explorador se redirige a la dirección URL de página de Error personalizado  
 ([Haga clic aquí para ver imagen en tamaño completo](processing-unhandled-exceptions-vb/_static/image12.png))

El efecto neto es que la solicitud donde se produjo la excepción no controlada finaliza cuando el servidor responde con la redirección HTTP 302. La solicitud posterior a la página de error personalizado es una solicitud nueva; por este punto ASP.NET motor ha descartado la información de error y, además, no tiene forma de asociar la excepción no controlada en la solicitud anterior a la nueva solicitud de página de error personalizada. Por ello `GetLastError` devuelve `null` cuando se llama desde la página de error personalizado.

Sin embargo, es posible que la página de error personalizada que se ejecuta durante la misma solicitud que produjo el error. El [ `Server.Transfer(url)` ](https://msdn.microsoft.com/library/system.web.httpserverutility.transfer.aspx) método transfiere la ejecución a la dirección URL especificada y lo procesa en la misma solicitud. Puede mover el código el `Application_Error` controlador de eventos para la clase de código subyacente de la página de error personalizado, reemplazarlo en `Global.asax` con el código siguiente:

[!code-vb[Main](processing-unhandled-exceptions-vb/samples/sample5.vb)]

Ahora cuando se produce una excepción no controlada del `Application_Error` controlador de eventos transfiere el control a la página de error personalizado apropiado en función del código de estado HTTP. Dado que el control se transfiere, la página de error personalizada tiene acceso a la información de excepción no controlada mediante `Server.GetLastError` y puede notificar a un desarrollador del error y sus detalles de registro. El `Server.Transfer` llamada detiene el motor de ASP.NET de redirigir al usuario a la página de error personalizado. En su lugar, se devuelve el contenido de la página de error personalizado como respuesta a la página que ha generado el error.

## <a name="summary"></a>Resumen

Cuando se produce una excepción no controlada en una aplicación web ASP.NET el tiempo de ejecución ASP.NET genera el `Error` eventos y muestra la página de error configurado. Podemos enviarle notificaciones el desarrollador del error, sus detalles de registro, o quiere procesarla de algún otro modo, mediante la creación de un controlador de eventos para el evento de Error. Hay dos maneras de crear un controlador de eventos `HttpApplication` eventos, como `Error`: en el `Global.asax` archivo o desde un módulo HTTP. Este tutorial ha mostrado cómo crear un `Error` controlador de eventos en el `Global.asax` archivo que notifica a los desarrolladores de un error por medio de un mensaje de correo electrónico.

Creación de un `Error` es útil si necesita procesar excepciones no controladas de alguna manera personalizada o único controlador de eventos. Sin embargo, crear su propio `Error` controlador de eventos para registrar la excepción o para notificar a un desarrollador no es el uso más eficaz de su tiempo como ya existen bibliotecas de registro de error gratuita y fácil de usar que pueden configurarse en cuestión de minutos. Los siguientes dos tutoriales examinan dos de estas bibliotecas.

Feliz programación.

### <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas tratados en este tutorial, consulte los siguientes recursos:

- [Introducción a los controladores y módulos HTTP de ASP.NET](https://support.microsoft.com/kb/307985)
- [Responder correctamente a las excepciones no controladas - procesar excepciones no controladas](http://aspnet.4guysfromrolla.com/articles/091306-1.aspx)
- [`HttpApplication` Clase y el objeto de aplicación de ASP.NET](http://www.eggheadcafe.com/articles/20030211.asp)
- [Los controladores HTTP y módulos HTTP de ASP.NET](http://www.15seconds.com/Issue/020417.htm)
- [Envío de correo electrónico en ASP.NET](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx)
- [Descripción de la `Global.asax` archivo](http://aspalliance.com/1114_Understanding_the_Globalasax_file.all)
- [Uso de módulos y controladores HTTP para crear componentes ASP.NET conectables](https://msdn.microsoft.com/library/aa479332.aspx)
- [Trabajar con ASP.NET `Global.asax` archivo](http://articles.techrepublic.com.com/5100-10878_11-5771721.html)
- [Trabajar con `HttpApplication` instancias](https://msdn.microsoft.com/library/a0xez8f2.aspx)

> [!div class="step-by-step"]
> [Anterior](displaying-a-custom-error-page-vb.md)
> [Siguiente](logging-error-details-with-asp-net-health-monitoring-vb.md)
