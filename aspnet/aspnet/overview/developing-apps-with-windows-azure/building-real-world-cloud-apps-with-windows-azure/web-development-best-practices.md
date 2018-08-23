---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices
title: Web Development Best Practices (crear aplicaciones de nube reales con Azure) | Microsoft Docs
author: MikeWasson
description: Building Real World Cloud Apps with e-book de Azure se basa en una presentación desarrollada por Scott Guthrie. Explican el 13 de patrones y prácticas que puede...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: 52d6c941-2cd9-442f-9872-2c798d6d90cd
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices
msc.type: authoredcontent
ms.openlocfilehash: be014eabcc7f88f229722b2d565262c9025c5ad3
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829028"
---
<a name="web-development-best-practices-building-real-world-cloud-apps-with-azure"></a>Prácticas recomendadas de desarrollo de Web (creación de aplicaciones de nube reales con Azure)
====================
por [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Descargar proyecto corregirlo](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) o [descargar libro electrónico](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> El **Building Real World Cloud Apps with Azure** eBook se basa en una presentación desarrollada por Scott Guthrie. Se explican el 13 patrones y prácticas que pueden ayudarle a tener éxito el desarrollo de aplicaciones web para la nube. Para obtener información sobre el libro electrónico, consulte [el primer capítulo](introduction.md).


Los tres primeros patrones eran acerca de cómo configurar un proceso de desarrollo ágil; el resto son sobre arquitectura y código. Ésta es una colección de prácticas recomendadas de desarrollo web:

- [Los servidores web sin estado](#stateless) detrás de un equilibrador de carga inteligente.
- [Evitar el estado de sesión](#sessionstate) (o si no se puede evitar, utilizar caché distribuida en lugar de una base de datos).
- [Usar una red CDN](#cdn) en caché de borde de los recursos de archivos estáticos (imágenes, scripts).
- [Usar la compatibilidad de async de .NET 4.5](#async) para evitar el bloqueo de llamadas.

Estas prácticas son válidas para todo el desarrollo web, no solo para aplicaciones de nube, pero son especialmente importantes para las aplicaciones de nube. Trabajan juntos para ayudarle a hacer un uso óptimo de la escala muy flexible que ofrece el entorno de nube. Si no sigue estas prácticas, podrá ejecutar en limitaciones al intentar escalar la aplicación.

<a id="stateless"></a>
## <a name="stateless-web-tier-behind-a-smart-load-balancer"></a>Nivel de web sin estado detrás de un equilibrador de carga inteligente

*Nivel web sin estado* significa que no almacene los datos de la aplicación en el sistema de archivo o la memoria de servidor web. Mantener su nivel web sin estado permite ahorrar dinero y ofrecer una mejor experiencia de cliente:

- Si el nivel web no tiene estado y que se encuentra detrás de un equilibrador de carga, puede responder rápidamente a los cambios en el tráfico de la aplicación agregando o quitando los servidores de forma dinámica. En el entorno de nube donde solo paga por recursos de servidor para siempre los use realmente, esa capacidad para responder a cambios en la demanda puede traducir en un ahorro enorme.
- Un nivel web sin estado es arquitectónicamente mucho más fácil de escalar horizontalmente la aplicación. Que también le permite responder a necesidades de escalado más rápidamente, y ahorrar dinero en desarrollo y pruebas en el proceso.
- Servidores en la nube, como los servidores locales, deben revisarse y reiniciar de forma ocasional; y si el nivel web no tiene estado, redirigir el tráfico cuando un servidor deja de funcionar temporalmente no causará errores o comportamientos inesperados.

La mayoría de las aplicaciones reales es necesario almacenar el estado de una sesión web; el principal punto es no almacenar en el servidor web. Puede almacenar el estado de otras maneras, como en el cliente en las cookies o fuera de proceso del lado servidor en estado de sesión ASP.NET mediante un proveedor de caché. Puede almacenar archivos en [Windows Azure Blob storage](unstructured-blob-storage.md) en lugar del sistema de archivos local.

Como ejemplo de lo fácil que es escalar una aplicación en sitios Web de Windows Azure si su nivel web no tiene estado, consulte el **escala** pestaña para un sitio Web de Windows Azure en el portal de administración:

![Pestaña escala](web-development-best-practices/_static/image1.png)

Si desea agregar servidores web, simplemente puede arrastrar el control deslizante de recuento de instancia a la derecha. Establézcalo en 5 y haga clic en **guardar**, y en cuestión de segundos tiene 5 servidores web en el control de tráfico del sitio web de Windows Azure.

![Cinco instancias](web-development-best-practices/_static/image2.png)

Puede establecer fácilmente el recuento de instancias hasta 3 o volver a 1. Cuando se reduzca, empieza a ahorrar inmediatamente como Windows Azure cobra por minuto, no por hora.

También puede indicar a Windows Azure para aumentar o disminuir el número de servidores web basados en el uso de CPU automáticamente. En el ejemplo siguiente, cuando el uso de CPU esté por debajo de 60%, se reducirá el número de servidores web en un mínimo de 2 y, si el uso de CPU supera el 80%, aumentará el número de servidores web hasta un máximo de 4.

![Escalar mediante el uso de CPU](web-development-best-practices/_static/image3.png)

O bien, ¿qué ocurre si sabe que su sitio solo será ocupado durante horas de trabajo? Puede indicar a Windows Azure para ejecutar varios servidores durante el día y reducir a un único servidor por la noche, nights y fines de semana. La siguiente serie de capturas de pantalla muestra cómo configurar el sitio web para ejecutar un servidor en fuera del horario laboral y 4 servidores durante el horario de trabajo de 8 A.M. a 5 P.M.

![Escalar por programación](web-development-best-practices/_static/image4.png)

![Establecer los tiempos de programación](web-development-best-practices/_static/image5.png)

![Programación durante el día](web-development-best-practices/_static/image6.png)

![Programación weeknight](web-development-best-practices/_static/image7.png)

![Programación de fin de semana](web-development-best-practices/_static/image8.png)

Y por supuesto todo esto puede hacerse en secuencias de comandos, así como en el portal.

La capacidad de escalar horizontalmente la aplicación es casi ilimitada en Windows Azure, siempre y cuando evitar obstáculos a dinámicamente agregando o quitando máquinas virtuales de servidor, manteniendo el nivel web sin estado.

<a id="sessionstate"></a>
## <a name="avoid-session-state"></a>Evitar el estado de sesión

A menudo no resulta práctico en una aplicación de nube del mundo real para evitar almacenar algún tipo de estado para una sesión de usuario, pero algunos enfoques afectan al rendimiento y escalabilidad más que otras. Si tiene que almacenar el estado, la mejor solución es mantener la cantidad de estado pequeño y almacene en cookies. Si esto no es factible, entonces lo mejor es utilizar el estado de sesión ASP.NET con un proveedor para [caché distribuida en memoria](distributed-caching.md#sessionstate). La peor solución desde un punto de vista de escalabilidad y rendimiento es usar una base de datos respaldada de proveedor de estado de sesión.

<a id="cdn"></a>
## <a name="use-a-cdn-to-cache-static-file-assets"></a>Usar una red CDN para almacenar en caché los activos de archivos estáticos

Red CDN es un acrónimo de Content Delivery Network. Proporcionar recursos de archivos estáticos como imágenes y archivos de script a un proveedor de CDN y el proveedor almacena en caché estos archivos en los centros de datos de todo el mundo para que siempre que los usuarios tener acceso a la aplicación, reciben respuesta relativamente rápida y latencia baja para el almacenado en caché activos. Esto acelera el tiempo de carga global del sitio y reduce la carga en los servidores web. Las redes CDN son especialmente importantes si están llegando a una audiencia que es ampliamente distribuida geográficamente.

Windows Azure tiene una red CDN, y puede usar otras CDN en una aplicación que se ejecuta en Windows Azure o en cualquier entorno de hospedaje web.

<a id="async"></a>
## <a name="use-net-45s-async-support-to-avoid-blocking-calls"></a>Utilice soporte para asincronía de .NET 4.5 para evitar el bloqueo de llamadas

.NET 4.5 mejorado los lenguajes de programación de C# y VB para que resulte mucho más fácil de administrar las tareas de forma asincrónica. La ventaja de la programación asincrónica no es solo para situaciones de procesamiento en paralelo, como cuando desee iniciar varias llamadas de servicio web al mismo tiempo. También permite su servidor web para realizar de manera más eficaz y confiable en condiciones de carga alta. Un servidor web tiene solo un número limitado de subprocesos disponibles y en condiciones de carga alta cuando todos los subprocesos están en uso, las solicitudes entrantes tengan que esperar hasta que los subprocesos se liberan. Si el código de aplicación no controla las tareas como la base de datos las consultas y llamadas al servicio web asincrónicamente, acumular muchos subprocesos son innecesariamente mientras el servidor está esperando una respuesta de E/S. Esto limita la cantidad de tráfico, que el servidor puede controlar en condiciones de carga alta. Programación asincrónica, se liberan los subprocesos que están esperando para que un servicio web o una base de datos para devolver los datos hasta atender nuevas solicitudes hasta que los datos la recibe. En un servidor web ocupados, cientos o miles de solicitudes, a continuación, se pueden procesar con prontitud que estarían esperando para que los subprocesos se libere.

Como vimos anteriormente, es tan fácil reducir el número de servidores web control de su sitio web sea aumentarlas. Por lo que si un servidor puede lograr un rendimiento mayor, no necesita que muchos de ellos y pueden reducir los costos porque necesita menos servidores de un volumen de tráfico determinado a como lo haría en caso contrario.

Compatibilidad con el modelo de programación asincrónico de .NET 4.5 se incluye en ASP.NET 4.5 para formularios Web Forms, MVC y Web API; en Entity Framework 6 y en el [API de Windows Azure Storage](https://blogs.msdn.com/b/windowsazurestorage/archive/2013/07/12/introducing-storage-client-library-2-1-rc-for-net-and-windows-phone-8.aspx).

### <a name="async-support-in-aspnet-45"></a>Soporte para asincronía en ASP.NET 4.5

En ASP.NET 4.5, la compatibilidad con programación asincrónica se ha agregado, no solo para el idioma, sino también a los marcos MVC, formularios Web Forms y Web API. Por ejemplo, un método de acción de controlador de MVC de ASP.NET recibe los datos de una solicitud web y pasa los datos a una vista que, a continuación, crea el código HTML que se envía al explorador. El método de acción necesita con frecuencia obtener datos desde una base de datos o servicio web para mostrarlo en una página web o para guardar los datos escritos en una página web. En estos escenarios es fácil de hacer que el método de acción asincrónico: en lugar de devolver un *ActionResult* objeto, devuelven *tarea&lt;ActionResult&gt;*  y marque el método con el *async* palabra clave. Dentro del método, cuando una línea de código se inicia una operación que implica el tiempo de espera, se marca con la palabra clave await.

Este es un método de acción simple que llama a un método de repositorio para una consulta de base de datos:

[!code-csharp[Main](web-development-best-practices/samples/sample1.cs)]

Y aquí es el mismo método que controla la llamada de la base de datos de forma asincrónica:

[!code-csharp[Main](web-development-best-practices/samples/sample2.cs?highlight=1,4)]

En segundo plano, el compilador genera el código asincrónico adecuado. Cuando la aplicación realiza la llamada a `FindTaskByIdAsync`, ASP.NET hace que el `FindTask` solicitar y, a continuación, el subproceso de trabajo se desenreda y hace que esté disponible para procesar otra solicitud. Cuando el `FindTask` se realiza la solicitud, se reinicia un subproceso para continuar procesando el código que viene después de esa llamada. Durante la versión preliminar entre cuando el `FindTask` solicitud se inicia y cuando se devuelven los datos, tiene un subproceso disponible para realizar trabajo útil que de otro modo podría acumular espera la respuesta.

Hay cierta sobrecarga para código asincrónico, pero en condiciones de poca carga, dicha sobrecarga es insignificante, mientras que en condiciones de carga alta que puede procesar las solicitudes que de lo contrario se mantendrían la espera de los subprocesos disponibles.

Ha sido posible hacer este tipo de programación asincrónica desde ASP.NET 1.1, pero era difícil de escribir, propensas a errores y difícil de depurar. Ahora que hemos simplificado la codificación para él en ASP.NET 4.5, no hay ninguna razón para no hacerlo ya.

### <a name="async-support-in-entity-framework-6"></a>Soporte para asincronía en Entity Framework 6

Como parte del soporte para asincronía en 4.5 se incluye soporte para asincronía para llamadas a servicios web sockets y sistema de archivos de E/S, pero el modelo más común para las aplicaciones web es una base de datos de posicionamiento y nuestras bibliotecas de datos no admitían async. Ahora, Entity Framework 6 agrega compatibilidad asincrónica para el acceso a la base de datos.

En Entity Framework 6 todos los métodos que hacen que una consulta o comando que se enviará a la base de datos tienen versiones asincrónicas. En este ejemplo se muestra la versión asincrónica de la *buscar* método.

[!code-csharp[Main](web-development-best-practices/samples/sample3.cs?highlight=8)]

Y este soporte para asincronía funciona no solo para las inserciones, eliminaciones, actualizaciones y busca simple, también funciona con consultas LINQ:

[!code-csharp[Main](web-development-best-practices/samples/sample4.cs?highlight=7,10)]

Hay un `Async` versión de la `ToList` método porque en este código, que es el método que hace que una consulta para enviarse a la base de datos. El `Where` y `OrderByDescending` métodos configuración solo la consulta, mientras que el `ToListAsync` método se ejecuta la consulta y almacena la respuesta en el `result` variable.

## <a name="summary"></a>Resumen

Puede implementar las prácticas recomendadas de desarrollo de web que se describen aquí en cualquier marco de trabajo y cualquier entorno de nube de programación web, pero tenemos las herramientas de ASP.NET y Windows Azure para que resulte sencillo. Si sigue estos patrones, puede escalar horizontalmente su capa web y a minimizar los gastos porque cada servidor pueda administrar más tráfico.

El [siguiente capítulo](single-sign-on.md) examina cómo la nube permite escenarios de inicio de sesión único.

## <a name="resources"></a>Recursos

Para obtener más información, consulte los siguientes recursos.

Servidores web sin estado:

- [Microsoft Patterns and Practices - Guía de escalado automático](https://msdn.microsoft.com/library/dn589774.aspx).
- [Deshabilitación ARR instancia afinidad en Microsoft Azure websites](https://azure.microsoft.com/blog/2013/11/18/disabling-arrs-instance-affinity-in-windows-azure-web-sites/). Entrada de blog de Erez Benari, explica la afinidad de sesión en sitios Web de Windows Azure.

RED CDN:

- [FailSafe: Creación de servicios de nube escalables, resistentes](https://channel9.msdn.com/Series/FailSafe). Serie de vídeos de nueve partes, Ulrich Homann, Marc Mercuri y Mark Simms. Vea la explicación de la red CDN episodio 3 empieza en 1:34:00.
- [Patrón Microsoft Patterns and Practices Static Content Hosting](https://msdn.microsoft.com/library/dn589776.aspx)
- [Las revisiones de la red CDN](http://www.cdnreviews.com/). Información general de muchas de las redes CDN.

Programación asincrónica:

- [Usar métodos asincrónicos en ASP.NET MVC 4](../../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md). Tutorial de Rick Anderson.
- [Programación asincrónica con Async y Await (C# y Visual Basic)](https://msdn.microsoft.com/library/vstudio/hh191443.aspx). Documento de MSDN que explica el fundamento de la programación asincrónica, cómo funciona en ASP.NET 4.5 y cómo escribir código para implementarlo.
- [Consulta de Entity Framework asincrónicas y guardar](https://msdn.microsoft.com/data/jj819165)
- [Cómo crear aplicaciones Web ASP.NET utilizando Async](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2013/DEV-B337#fbid=tgkT4SR_DK7). Presentación en vídeo de Rowan Miller. Incluye una demostración de gráfico de la programación asincrónica en cómo puede facilitar aumentar el rendimiento del servidor web en condiciones de carga alta.
- [FailSafe: Creación de servicios de nube escalables, resistentes](https://channel9.msdn.com/Series/FailSafe). Serie de vídeos de nueve partes, Ulrich Homann, Marc Mercuri y Mark Simms. Discusiones sobre el impacto de la programación asincrónica en la escalabilidad, consulte episodio 4 y 8 episodio.
- [La magia de uso de métodos asincrónicos en ASP.NET 4.5 además de un problema importante](http://www.hanselman.com/blog/TheMagicOfUsingAsynchronousMethodsInASPNET45PlusAnImportantGotcha.aspx). Blog de Scott Hanselman, principalmente sobre el uso de async en las aplicaciones de formularios Web Forms de ASP.NET.

Para las prácticas recomendadas de desarrollo de web adicionales, consulte los siguientes recursos:

- [La corrección de aplicación: prácticas recomendadas de ejemplo](the-fix-it-sample-application.md#bestpractices). El apéndice de este libro electrónico enumera una serie de prácticas recomendadas que se han implementado en la aplicación Fix It.
- [Lista de comprobación de Web para desarrolladores](http://webdevchecklist.com/asp.net)

> [!div class="step-by-step"]
> [Anterior](continuous-integration-and-continuous-delivery.md)
> [Siguiente](single-sign-on.md)
