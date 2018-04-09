---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices
title: Web prácticas recomendadas de desarrollo (creación de aplicaciones de nube reales con Azure) | Documentos de Microsoft
author: MikeWasson
description: Las aplicaciones de nube de creación Real World con libros electrónicos Azure se basa en una presentación desarrollada por Scott Guthrie. Se explican 13 patrones y prácticas recomendadas que puede...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: 52d6c941-2cd9-442f-9872-2c798d6d90cd
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices
msc.type: authoredcontent
ms.openlocfilehash: 4c43b256018d91e89b3427f90fc5c6cd018641f9
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="web-development-best-practices-building-real-world-cloud-apps-with-azure"></a>Prácticas recomendadas de desarrollo de Web (creación de aplicaciones de nube reales con Azure)
====================
por [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Descarga corregirlo proyecto](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) o [descargar libros electrónicos](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> El **compilar aplicaciones de nube de mundo Real con Azure** libros electrónicos se basa en una presentación desarrollada por Scott Guthrie. Se explican los 13 patrones y prácticas recomendadas que le permitirá tener éxito el desarrollo de aplicaciones web para la nube. Para obtener información sobre el libro electrónico, consulte [el primer capítulo](introduction.md).


Los tres primeros patrones estaban acerca de cómo configurar un proceso de desarrollo ágil; el resto son sobre arquitectura y el código. Esta es una colección de prácticas recomendadas de desarrollo de web:

- [Servidores web sin estado](#stateless) detrás de un equilibrador de carga inteligente.
- [Evitar el estado de sesión](#sessionstate) (o si no se pueden evitar, use caché distribuida en lugar de una base de datos).
- [Utilice una CDN](#cdn) a los recursos de archivos estáticos de caché perimetral (imágenes, las secuencias de comandos).
- [Usar acepta el funcionamiento asincrónico de .NET 4.5](#async) para evitar el bloqueo de llamadas.

Estos procedimientos son válidos para todo el desarrollo web, no solo para las aplicaciones de nube, pero son especialmente importantes para las aplicaciones de nube. Trabajan juntos para ayudarle a hacer un uso óptimo de la escala muy flexible que ofrece el entorno de nube. Si no sigue estas prácticas, ejecutaremos frente a las limitaciones al intentar escalar la aplicación.

<a id="stateless"></a>
## <a name="stateless-web-tier-behind-a-smart-load-balancer"></a>Nivel de web sin estado detrás de un equilibrador de carga inteligente

*Nivel web sin estado* significa no almacenar datos de aplicaciones en el sistema de memoria o en archivos de servidor web. Mantener el nivel de web sin estado permite ofrecer una mejor experiencia de cliente y ahorrar dinero:

- Si el nivel de web no tiene estado y se encuentra detrás de un equilibrador de carga, puede responder rápidamente a los cambios en el tráfico de la aplicación mediante Agregar o quitar servidores de forma dinámica. En el entorno de nube donde solo se paga por recursos del servidor para siempre y cuando utilizas realmente, esa capacidad para responder a cambios en la demanda se traduce en gran ahorro.
- Un nivel de web sin estado es su arquitectura mucho más fácil de escalar horizontalmente la aplicación. Demasiado que permite responder a las necesidades de ajuste de escala más rápidamente y ahorrar dinero en desarrollo y pruebas en el proceso.
- Servidores de la nube, como servidores locales, se deben revisar y reinicia ocasionalmente; y si el nivel de web no tiene estado, reenrutamiento tráfico cuando un servidor deja de funcionar temporalmente no provocará errores o un comportamiento inesperado.

La mayoría de las aplicaciones reales es necesario almacenar el estado de una sesión web; es el principal punto no se va a almacenar en el servidor web. Puede almacenar el estado de otras maneras, como en el cliente en las cookies o fuera de proceso del servidor en estado de sesión ASP.NET usando un proveedor de caché. Puede almacenar los archivos en [almacenamiento de blobs de Windows Azure](unstructured-blob-storage.md) en lugar del sistema de archivos local.

Como ejemplo de lo fácil que es escalar una aplicación en sitios Web de Windows Azure si el nivel de web no tiene estado, consulte la **escala** ficha para un sitio Web de Windows Azure en el portal de administración:

![Pestaña escalar](web-development-best-practices/_static/image1.png)

Si desea agregar servidores web, simplemente puede arrastrar el control deslizante de recuento de instancia a la derecha. Establece en 5 y haga clic en **guardar**, y en segundos tiene 5 servidores web en el control de tráfico de su sitio web de Windows Azure.

![Cinco instancias](web-development-best-practices/_static/image2.png)

Igual de sencillo, puede establecer el recuento de instancias hasta 3 o volver a 1. Cuando se reduzca, iniciar ahorra dinero inmediatamente como Windows Azure cobra por minuto, no por horas.

También puede indicar a Windows Azure para aumentar o reducir el número de servidores web basada en el uso de CPU automáticamente. En el ejemplo siguiente, cuando el uso de CPU queda por debajo del 60%, se reducirá el número de servidores web a un mínimo de 2 y, si el uso de CPU supera el 80%, el número de servidores web se incrementará hasta un máximo de 4.

![Escalar mediante el uso de CPU](web-development-best-practices/_static/image3.png)

O bien, ¿qué ocurre si sabe que el sitio sólo estará ocupado durante el horario laboral? Puede indicar a Windows Azure para ejecutar varios servidores durante el día y reducir a un único servidor por la noche, nights y fines de semana. La siguiente serie de capturas de pantalla muestra cómo configurar el sitio web para ejecutar un servidor en horas de poca actividad y 4 servidores durante las horas laborables de 8 A.M. a 5 P.M.

![Escalar mediante programación](web-development-best-practices/_static/image4.png)

![Establecer los tiempos de programación](web-development-best-practices/_static/image5.png)

![Programación de día](web-development-best-practices/_static/image6.png)

![Programación de weeknight](web-development-best-practices/_static/image7.png)

![Programación de fin de semana](web-development-best-practices/_static/image8.png)

Y por supuesto todo esto puede realizarse en secuencias de comandos, así como en el portal.

La capacidad de la aplicación para escalar horizontalmente es casi ilimitada en Windows Azure, siempre y cuando evitar impedimentos para dinámicamente agregando o quitando las máquinas virtuales de servidor, manteniendo el nivel web sin estado.

<a id="sessionstate"></a>
## <a name="avoid-session-state"></a>Evitar el estado de sesión

A menudo no resulta práctico en una aplicación de nube real evitar almacenar algún tipo de estado para una sesión de usuario, pero algunos métodos afectan al rendimiento y la escalabilidad más que otros. Si tiene que almacenar el estado, la mejor solución es mantener pequeña la cantidad de estado y almacenarlo en cookies. Si eso no es factible, la siguiente mejor solución consiste en utilizar el estado de sesión ASP.NET con un proveedor para [caché distribuida en memoria](distributed-caching.md#sessionstate). La peor solución desde un punto de vista de escalabilidad y rendimiento es usar una base de datos de copia de proveedor de estado de sesión.

<a id="cdn"></a>
## <a name="use-a-cdn-to-cache-static-file-assets"></a>Utilice una CDN para almacenar en caché los recursos de archivos estáticos

CDN es un acrónimo de red de entrega de contenido. Proporcionar recursos de archivos estáticos, como imágenes y archivos de script a un proveedor de red CDN, y el proveedor almacena en caché estos archivos en centros de datos distribuidas por todo el mundo para que siempre que los usuarios accedan a la aplicación, obtienen respuesta relativamente rápida y baja latencia para las almacenadas en caché activos. Esto acelera el tiempo de carga global del sitio y reduce la carga en los servidores web. CDN es especialmente importantes si están llegando a una audiencia que es ampliamente distribuida geográficamente.

Windows Azure tiene una CDN, y puede usar otros CDN en una aplicación que se ejecuta en Windows Azure o en cualquier entorno de hospedaje web.

<a id="async"></a>
## <a name="use-net-45s-async-support-to-avoid-blocking-calls"></a>Usar acepta el funcionamiento asincrónico de .NET 4.5 para evitar el bloqueo de llamadas

.NET 4.5 mejorado los lenguajes de programación de C# y VB para que sea mucho más fácil de administrar las tareas de forma asincrónica. La ventaja de la programación asincrónica no es solo para situaciones de procesamiento en paralelo, como cuando desee iniciar varias llamadas a servicios web al mismo tiempo. También permite el servidor web para realizar de forma más eficaz y confiable en condiciones de carga alta. Un servidor web solo tiene un número limitado de subprocesos disponibles y en condiciones de carga alta cuando todos los subprocesos están en uso, las solicitudes entrantes tienen que esperar hasta que se liberan subprocesos. Si el código de aplicación no controla las tareas como la base de datos las consultas y llamadas al servicio web asincrónicamente, muchos subprocesos se acumular innecesariamente la mientras el servidor está esperando una respuesta de E/S. Esto limita la cantidad de tráfico que puede administrar el servidor en condiciones de carga alta. Con la programación asincrónica, los subprocesos que están esperando para que un servicio web o una base de datos para devolver los datos se liberan hasta atender nuevas solicitudes hasta que los datos la recibe. En un servidor web ocupados, cientos o miles de solicitudes, a continuación, se pueden procesar rápidamente que en caso contrario, estarían esperando para que los subprocesos para ser liberados automáticamente.

Como se vio anteriormente, resulta tan sencillo reducir el número de servidores web que controlen el sitio web que resulta aumentarlas. Por lo que si un servidor puede lograr un mayor rendimiento, no necesita porque muchas de ellas y pueden reducir los costos porque necesita menos servidores de un volumen de tráfico determinado que lo haría en caso contrario.

Soporte para el modelo de programación asincrónico de .NET 4.5 se incluye en ASP.NET 4.5 para formularios Web Forms, MVC y API de Web; en Entity Framework 6 y en el [API de almacenamiento de Azure Windows](https://blogs.msdn.com/b/windowsazurestorage/archive/2013/07/12/introducing-storage-client-library-2-1-rc-for-net-and-windows-phone-8.aspx).

### <a name="async-support-in-aspnet-45"></a>Compatibilidad asincrónica en ASP.NET 4.5

En ASP.NET 4.5, compatibilidad con programación asincrónica se ha agregado no solo para el idioma, sino también a los marcos de MVC, formularios Web Forms y API Web. Por ejemplo, un método de acción de controlador de MVC de ASP.NET recibe los datos de una solicitud web y pasa los datos a una vista que, a continuación, crea el código HTML que se envía al explorador. Con frecuencia, el método de acción debe obtener datos de un base de datos o un servicio web para que se muestren en una página web o para guardar los datos escritos en una página web. En esos escenarios es fácil crear el método de acción asincrónico: en lugar de devolver un *ActionResult* de objeto, devuelven *tarea&lt;ActionResult&gt;*  y marque el método con el *async* palabra clave. Dentro del método, cuando una línea de código inicia una operación que implica el tiempo de espera, se marca con la palabra clave await.

Éste es un método de acción simple que llama a un método de repositorio para una consulta de base de datos:

[!code-csharp[Main](web-development-best-practices/samples/sample1.cs)]

Y aquí es el mismo método que controla la llamada de la base de datos de forma asincrónica:

[!code-csharp[Main](web-development-best-practices/samples/sample2.cs?highlight=1,4)]

En segundo plano, el compilador genera el código asincrónico adecuado. Cuando la aplicación realiza la llamada a `FindTaskByIdAsync`, ASP.NET realiza el `FindTask` solicitar y, a continuación, se desenreda el subproceso de trabajo y hace que esté disponible para procesar otra solicitud. Cuando el `FindTask` se realiza la solicitud, se reinicia un subproceso para continuar procesando el código que va después de esa llamada. Durante ese período entre el `FindTask` solicitud se inicia y cuando se devuelven los datos, tiene un subproceso disponible para realizar un trabajo útil que de otro modo, se podría acumular la espera para la respuesta.

Hay cierta sobrecarga de código asincrónico, pero en condiciones de poca carga, esa sobrecarga es insignificante, mientras en condiciones de carga elevada que puede procesar las solicitudes que en caso contrario, se puede mantener la espera de subprocesos disponibles.

Ha sido posible realizar este tipo de programación asincrónica desde la versión 1.1 de ASP.NET, pero es difícil de escribir, propensa a errores y difícil de depurar. Ahora que hemos simplificado la codificación para él en ASP.NET 4.5, no hay ninguna razón para no hacerlo ya.

### <a name="async-support-in-entity-framework-6"></a>Compatibilidad asincrónica en Entity Framework 6

Como parte de la compatibilidad asincrónica en 4.5 se incluye compatibilidad con async llamadas al servicio web, sockets y sistema de archivos E/S, pero el modelo más común para las aplicaciones web es se alcanza una base de datos y nuestro bibliotecas de datos no admiten asincrónico. Ahora, Entity Framework 6 agrega compatibilidad asincrónica para el acceso a la base de datos.

En Entity Framework 6 todos los métodos que hacen que una consulta o comando que se enviará a la base de datos tienen versiones asincrónicas. Este ejemplo muestra la versión asincrónica de la *buscar* método.

[!code-csharp[Main](web-development-best-practices/samples/sample3.cs?highlight=8)]

Y funciona esta compatibilidad asincrónica no solo para inserciones, eliminaciones, actualizaciones y busca simple, también funciona con consultas LINQ:

[!code-csharp[Main](web-development-best-practices/samples/sample4.cs?highlight=7,10)]

Hay un `Async` versión de la `ToList` método porque en este código, que es el método que hace que una consulta para enviarse a la base de datos. El `Where` y `OrderByDescending` métodos solo configure la consulta mientras el `ToListAsync` método se ejecuta la consulta y almacena la respuesta en el `result` variable.

## <a name="summary"></a>Resumen

Puede implementar las prácticas recomendadas de desarrollo de web que se describen aquí en cualquier marco de trabajo y cualquier entorno de nube de programación web, pero tenemos que herramientas de ASP.NET y Windows Azure que resulte sencillo. Si sigue estos patrones, puede escalar fácilmente el nivel de web y podrá minimizar los gastos ya que cada servidor puede administrar más tráfico.

El [siguiente capítulo](single-sign-on.md) examina cómo la nube permite escenarios de inicio de sesión único.

## <a name="resources"></a>Recursos

Para obtener más información, vea los siguientes recursos.

Servidores web sin estado:

- [Microsoft Patterns and Practices - Guía de escalado automático](https://msdn.microsoft.com/library/dn589774.aspx).
- [Deshabilitar ARR instancia afinidad en los sitios Web de Azure Windows](https://azure.microsoft.com/blog/2013/11/18/disabling-arrs-instance-affinity-in-windows-azure-web-sites/). Blog de Erez Benari, explica afinidad de la sesión en sitios Web de Windows Azure.

CDN:

- [FailSafe: Creación de servicios en la nube escalables y resistentes](https://channel9.msdn.com/Series/FailSafe). Serie de vídeos de nueve partes por Ulrich Homann y Marc Mercuri, Mark Simms. Vea la explicación de la red CDN episodio 3 empieza en 1:34:00.
- [Patrón de Microsoft Patterns y el hospedaje de contenido estático en prácticas recomendadas](https://msdn.microsoft.com/library/dn589776.aspx)
- [Revisiones de CDN](http://www.cdnreviews.com/). Información general de muchas CDN.

Programación asincrónica:

- [Usar métodos asincrónicos en ASP.NET MVC 4](../../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md). Tutorial de Rick Anderson.
- [Programación asincrónica con Async y Await (C# y Visual Basic)](https://msdn.microsoft.com/library/vstudio/hh191443.aspx). MSDN notas del producto que explica el motivo de la programación asincrónica, cómo funciona en ASP.NET 4.5 y cómo escribir código para implementarlo.
- [Consulta de Entity Framework asincrónico y guardar](https://msdn.microsoft.com/data/jj819165)
- [Cómo crear aplicaciones Web ASP.NET utilizando Async](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2013/DEV-B337#fbid=tgkT4SR_DK7). Presentación en vídeo Rowan Miller. Incluye una demostración de gráfico de la programación asincrónica cómo pueden facilitar aumenta significativamente el rendimiento del servidor web en condiciones de carga alta.
- [FailSafe: Creación de servicios en la nube escalables y resistentes](https://channel9.msdn.com/Series/FailSafe). Serie de vídeos de nueve partes por Ulrich Homann y Marc Mercuri, Mark Simms. Para debates sobre el impacto de la programación asincrónica en la escalabilidad, vea episodio de 4 y 8 del episodio.
- [La magia del uso de métodos asincrónicos en ASP.NET 4.5 más un problema importante](http://www.hanselman.com/blog/TheMagicOfUsingAsynchronousMethodsInASPNET45PlusAnImportantGotcha.aspx). Blog de Scott Hanselman, principalmente acerca de cómo usar async en aplicaciones de formularios Web Forms de ASP.NET.

Para prácticas recomendadas de desarrollo de web adicionales, consulte los siguientes recursos:

- [La corrección de ejemplo de aplicación: prácticas recomendadas](the-fix-it-sample-application.md#bestpractices). El apéndice de este libro electrónico enumera una serie de prácticas recomendadas que se implementaron en la aplicación repararlo.
- [Lista de comprobación de desarrollador Web](http://webdevchecklist.com/asp.net)

> [!div class="step-by-step"]
> [Anterior](continuous-integration-and-continuous-delivery.md)
> [Siguiente](single-sign-on.md)
