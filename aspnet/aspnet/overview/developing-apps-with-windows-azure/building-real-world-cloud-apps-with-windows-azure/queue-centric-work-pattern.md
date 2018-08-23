---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern
title: Patrón centrado en colas de trabajo (crear aplicaciones de nube reales con Azure) | Microsoft Docs
author: MikeWasson
description: Building Real World Cloud Apps with e-book de Azure se basa en una presentación desarrollada por Scott Guthrie. Explican el 13 de patrones y prácticas que puede...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: cc1ad51b-40c3-4c68-8620-9aaa0fd1f6cf
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern
msc.type: authoredcontent
ms.openlocfilehash: c454023c26d73df174a43b3a8ad6745ef8654fd2
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836611"
---
<a name="queue-centric-work-pattern-building-real-world-cloud-apps-with-azure"></a>Patrón centrado en colas de trabajo (crear aplicaciones de nube reales con Azure)
====================
por [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Descargar proyecto corregirlo](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) o [descargar libro electrónico](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> El **Building Real World Cloud Apps with Azure** eBook se basa en una presentación desarrollada por Scott Guthrie. Se explican el 13 patrones y prácticas que pueden ayudarle a tener éxito el desarrollo de aplicaciones web para la nube. Para obtener información sobre el libro electrónico, consulte [el primer capítulo](introduction.md).


Anteriormente, vimos que varios servicios de uso pueden producir un SLA "compuesto", donde es el SLA efectivo de la aplicación la *producto* de los SLA individuales. Por ejemplo, la aplicación Fix It usa sitios Web, almacenamiento y base de datos SQL. Si se produce un error en cualquiera de estos servicios, la aplicación devolverá un error al usuario.

Almacenamiento en caché es una buena forma de controlar los errores transitorios para contenido de sólo lectura. Pero ¿qué ocurre si la aplicación necesita para realizar el trabajo? Por ejemplo, cuando el usuario envía una nueva Fix It tarea, la aplicación no puede simplemente poner la tarea en la memoria caché. La aplicación debe escribir la tarea Fix It en un almacén de datos persistente, por lo que puede procesarse.

Que es donde entra en juego el patrón de trabajo centrado en colas. Este patrón permite un acoplamiento flexible entre un nivel web y un servicio back-end.

Así es cómo funciona el patrón. Cuando la aplicación recibe una solicitud, se coloca un elemento de trabajo en una cola y la respuesta se devuelve inmediatamente. A continuación, un proceso independiente de back-end extrae los elementos de trabajo de la cola y realiza el trabajo.

El patrón de trabajo centrado en la cola es útil para:

- Trabajo que es mucho tiempo (latencia alta).
- Trabajo que requiere un servicio externo que no siempre esté disponible.
- El trabajo que se consume muchos recursos (CPU alta).
- Trabajo que se beneficiaría de la tasa de redistribuir (sujeto a ráfagas repentinas de carga).

## <a name="reduced-latency"></a>Reducir la latencia

Las colas son útiles en cualquier momento que se están realizando trabajos que requieren mucho tiempo. Si una tarea tarda unos segundos o más, en lugar de bloquear al usuario final, coloca el elemento de trabajo en una cola. Indicar al usuario "Estamos trabajando en él" y, a continuación, usar un agente de escucha de cola para procesar la tarea en segundo plano.

Por ejemplo, si compra algo en un minorista en línea, el sitio web confirma su pedido inmediatamente. Pero eso no significa que las cosas ya está en un camión entregando. Coloca una tarea en una cola, y en segundo plano se lleva a cabo la comprobación de crédito, preparación de los elementos para su envío y así sucesivamente.

Para escenarios con una latencia corto, el tiempo total de extremo a otro puede ser mayor de uso de una cola, en comparación con realizando la tarea de forma sincrónica. Pero incluso entonces, las ventajas pueden superar ese desventaja.

## <a name="increased-reliability"></a>Mayor confiabilidad

En la versión de corregir lo que nos hemos visto hasta ahora, el front-end web está estrechamente con el back-end de la base de datos SQL. Si el servicio de base de datos SQL no está disponible, el usuario recibe un error. Si los reintentos no funcionan (es decir, el error es más de transitorio), lo único que puede hacer es mostrar un error y pedir al usuario que vuelva a intentarlo.

![Diagrama que muestra servidor web cliente con errores cuando se produce un error en la base de datos SQL back-end](queue-centric-work-pattern/_static/image1.png)

Uso de colas, cuando un usuario envía una tarea Fix It, la aplicación escribe un mensaje a la cola. La carga del mensaje es un [JSON](http://json.org/) representación de la tarea. Tan pronto como el mensaje se escribe en la cola, la aplicación devuelve y muestra inmediatamente un mensaje de confirmación al usuario.

Si cualquiera de los servicios de back-end, por ejemplo, la base de datos SQL o el agente de escucha de cola--queden sin conexión, los usuarios todavía pueden enviar nuevas tareas de Fix It. Los mensajes solo se pondrán en cola hasta que están disponibles los servicios de back-end nuevo. En ese momento, los servicios back-end se ponga al día en el trabajo pendiente.

![Diagrama que muestra web front-end continúe funcionando cuando se produce un error de base de datos de SQL](queue-centric-work-pattern/_static/image2.png)

Además, ahora puede agregar más lógica de back-end sin preocuparse por la resistencia de front-end. Por ejemplo, es posible que desee enviar un correo electrónico o mensaje SMS al propietario cada vez que se asigna un nuevo Fix It. Si el correo electrónico o un servicio de SMS deja de estar disponible, puede todo lo demás procesar y, a continuación, se coloca un mensaje en una cola independiente para enviar mensajes de correo electrónico o SMS.

Anteriormente, nuestro SLA efectivo era Web Apps &times; almacenamiento &times; SQL Database = 99,7%. (Consulte [diseño para sobrevivir a errores](design-to-survive-failures.md).)

Cuando se cambia la aplicación use una cola, el front-end web depende solo Web Apps y Storage, para un SLA del 99,8% compuesto. (Tenga en cuenta que las colas son parte del servicio de almacenamiento de Azure, por lo que se incluyen en el mismo SLA como almacenamiento de blobs).

Si necesita incluso mejor que 99,8%, puede crear dos colas en dos regiones diferentes. Designar uno como principal y el otro como secundario. En la aplicación, conmutación por error a la cola secundaria si la cola principal no está disponible. La posibilidad de que ambos estén disponibles al mismo tiempo es muy pequeña.

## <a name="rate-leveling-and-independent-scaling"></a>Redistribución de tarifa y escalado independiente

Las colas también son útiles para algo llamado *valorar la redistribución* o *nivelación de carga*.

Las aplicaciones Web a menudo son susceptibles a ráfagas repentinas de tráfico. Aunque puede usar el escalado automático para agregar automáticamente los servidores web para administrar el tráfico web mayor, es posible que el escalado automático no pueda reaccionar rápidamente a los picos abrupta de carga. Si los servidores web pueden descargar parte del trabajo que tienen que hacer mediante la escritura de un mensaje a una cola, puede administrar más tráfico. Un servicio back-end, a continuación, puede leer los mensajes de la cola y procesarlos. La profundidad de la cola se aumentar o reducir la medida que varía la carga entrante.

Con la mayor parte de su descarga desde un servicio back-end de trabajo que requieren mucho tiempo, el nivel web más fácilmente puede responder a picos repentinos de tráfico. Y ahorrar dinero, porque cualquier cantidad de tráfico determinado puede controlarse mediante menos servidores web.

Puede escalar el nivel web y el servicio de back-end de forma independiente. Por ejemplo, puede necesitar tres servidores web, pero solo mensajes de cola de procesamiento de servidor. O bien, si ejecuta una tarea de proceso intensivo en segundo plano, podría necesitar más servidores de back-end.

![](queue-centric-work-pattern/_static/image3.png)

El escalado automático funciona con servicios back-end, así como con el nivel de web. Puede escalar o reducir verticalmente el número de máquinas virtuales que se están procesando las tareas en la cola, en función del uso de CPU de las máquinas virtuales de back-end. O bien, puede que el escalado automático en función de cuántos elementos se encuentran en una cola. Por ejemplo, puede indicar el escalado automático para intentar mantener no más de 10 elementos en la cola. Si la cola tiene más de 10 elementos, el escalado automático agregará las máquinas virtuales. Cuando, a continuación, escalado automático anulará las máquinas virtuales adicionales.

## <a name="adding-queues-to-the-fix-it-application"></a>Agregarlo pone en cola para la corrección de aplicación

Para implementar el patrón de cola, es necesario realizar dos cambios en la aplicación Fix It.

- Cuando un usuario envía una nueva Fix It tarea, coloque la tarea en la cola, en lugar de escribirlos en la base de datos.
- Crear un servicio back-end que procesa los mensajes en la cola.

La cola, vamos a usar el [servicio Azure Queue Storage](https://www.windowsazure.com/develop/net/how-to-guides/queue-service/). Otra opción consiste en usar [Azure Service Bus](https://docs.microsoft.com/azure/service-bus/).

Para decidir qué servicio de cola que se usará, tenga en cuenta cómo la aplicación necesita para enviar y recibir los mensajes en la cola:

- Si tienes cooperativos productores y consumidores en competencia, considere el uso de servicio de Azure Queue Storage. "Productores de cooperación" significa que varios procesos son agregar mensajes a una cola. "Consumidores en competencia" significa que extraen mensajes de la cola procese varios procesos, pero solo se puede procesar cualquier mensaje determinado por un "consumidor". Si necesita más rendimiento del que se puede obtener con una sola cola, use colas adicionales o cuentas de almacenamiento adicional.
- Si necesita un [modelo de publicación/suscripción](http://en.wikipedia.org/wiki/Publish/subscribe), considere el uso de colas de Azure Service Bus.

La aplicación Fix It se ajusta a los productores de cooperación y el modelo competencia de consumidores.

Otra consideración es la disponibilidad de las aplicaciones. El servicio de Queue Storage es parte del mismo servicio que vamos a usar para almacenamiento de blobs, por lo que usarlo no tiene ningún efecto sobre nuestro SLA. Azure Service Bus es un servicio independiente con su propio SLA. Si se utilizan colas de Service Bus, se tendría que incluir en un porcentaje SLA adicional, y nuestro SLA compuesto sería menor. Cuando se elige un servicio de cola, asegúrese de que comprender el impacto de su elección en la disponibilidad de la aplicación. Para obtener más información, consulte el [recursos](#resources) sección.

## <a name="creating-queue-messages"></a>Creación de cola de mensajes

Para colocar una tarea Fix It en la cola, el front-end web lleva a cabo los pasos siguientes:

1. Crear un [CloudQueueClient](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueueclient.aspx) instancia. El `CloudQueueClient` instancia se usa para ejecutar solicitudes en el servicio cola.
2. Crear la cola, si aún no existe.
3. Serializar la tarea Fix It.
4. Llame a [CloudQueue.AddMessageAsync](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueue.addmessageasync.aspx) para colocar el mensaje en la cola.

Se llevará a cabo este trabajo en el constructor y `SendMessageAsync` un nuevo método `FixItQueueManager` clase.

[!code-csharp[Main](queue-centric-work-pattern/samples/sample1.cs?highlight=11-12,16,18-25)]

Aquí usamos el [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) library para serializar la corrección en formato JSON. Puede usar cualquier enfoque de serialización que prefiera. JSON tiene la ventaja de ser legible, al tiempo que es menos detallado que XML.

Código de calidad de producción sería agregar lógica de control de errores, pause Si dejó de estar disponible la base de datos, controlar la recuperación de forma más limpia, crear la cola en el inicio de la aplicación y administrar "[dudosos" mensajes](https://msdn.microsoft.com/library/ms789028(v=vs.110).aspx). (Un mensaje dudoso es un mensaje que no se puede procesar por algún motivo. No desea que los mensajes dudosos se colocan en la cola, donde el rol de trabajo continuamente intentará procesarlos, producirá un error, vuelva a intentarlo, producirá un error y así sucesivamente.)

En la aplicación de MVC front-end, deberá actualizar el código que crea una nueva tarea. En lugar de colocar la tarea en el repositorio, llame a la `SendMessageAsync` método mostrado anteriormente.

[!code-csharp[Main](queue-centric-work-pattern/samples/sample2.cs?highlight=10)]

## <a name="processing-queue-messages"></a>Procesamiento de mensajes en cola

Para procesar mensajes de la cola, vamos a crear un servicio back-end. El servicio de back-end se ejecutará un bucle infinito que lleva a cabo los pasos siguientes:

1. Obtiene el siguiente mensaje de la cola.
2. Deserializar el mensaje a una tarea Fix It.
3. Escribir la tarea Fix It en la base de datos.

Para hospedar el servicio back-end, vamos a crear un servicio en la nube de Azure que contiene un *rol de trabajo*. Un rol de trabajo consta de uno o más máquinas virtuales que pueden realizar el procesamiento de back-end. El código que se ejecuta en estas máquinas virtuales extraerán mensajes de la cola en cuanto estén disponibles. Para cada mensaje, se podrá deserializar la carga de JSON y escribir una instancia de la entidad de corregir su tarea a la base de datos con el mismo repositorio que se ha usado anteriormente en el nivel web.

Los pasos siguientes muestran cómo agregar un trabajo de proyecto de rol a una solución que tiene un proyecto web estándar. Estos pasos ya se han realizado en el proyecto Fix It que puede descargar.

En primer lugar, agregue un proyecto de servicio en la nube a la solución de Visual Studio. Haga clic en la solución y seleccione **agregar**, a continuación, **nuevo proyecto**. En el panel izquierdo, expanda **Visual C#** y seleccione **en la nube**.

[![](queue-centric-work-pattern/_static/image5.png)](queue-centric-work-pattern/_static/image4.png)

En el **nuevo servicio en la nube de Azure** cuadro de diálogo, expanda el **Visual C#** nodo en el panel izquierdo. Seleccione **rol de trabajo** y haga clic en el icono de flecha derecha.

![](queue-centric-work-pattern/_static/image6.png)

(Tenga en cuenta que también puede agregar un *rol web*. Podríamos ejecutamos el Fix It front-end en el mismo servicio en la nube en lugar de ejecutarlo en un sitio Web de Azure. Que tiene algunas ventajas en lo facilita coordinar las conexiones entre el front-end y back-end. Sin embargo, para simplificar esta demostración, estamos teniendo el front-end en una aplicación Web de Azure App Service y ejecuta solo el back-end en un servicio en la nube.)

Se asigna un nombre predeterminado para el rol de trabajo. Para cambiar el nombre, mantenga el mouse sobre el rol de trabajo en el panel derecho y luego haga clic en el icono de lápiz.

![](queue-centric-work-pattern/_static/image7.png)

Haga clic en **Aceptar** para completar el cuadro de diálogo. Esto agrega dos proyectos a la solución de Visual Studio.

- un proyecto de Azure que define el servicio en la nube, incluida la información de configuración.
- Un proyecto de rol de trabajo que define el rol de trabajo.

![](queue-centric-work-pattern/_static/image8.png)

Para obtener más información, consulte [crear un proyecto de Azure con Visual Studio.](https://msdn.microsoft.com/library/windowsazure/ee405487.aspx)

En el rol de trabajo, se sondean los mensajes de mediante una llamada a la `ProcessMessageAsync` método de la `FixItQueueManager` clase que vimos anteriormente.

[!code-csharp[Main](queue-centric-work-pattern/samples/sample3.cs?highlight=25)]

El `ProcessMessagesAsync` método comprueba si hay un mensaje en espera. Si hay alguno, deserializa el mensaje en un `FixItTask` entidad y guarda la entidad en la base de datos. Repite hasta que la cola está vacía.

[!code-csharp[Main](queue-centric-work-pattern/samples/sample4.cs)]

Sondeo de cola de mensajes incurre en una transacción pequeña carga, por lo que cuando no hay ningún mensaje que esperan ser procesados, el rol de trabajo `RunAsync` método espera un segundo antes de sondear de nuevo mediante una llamada a `Task.Delay(1000)`.

En un proyecto web, agregar código asincrónico automáticamente mejoran el rendimiento porque IIS administra un grupo de subprocesos limitado. No es el caso de un proyecto de rol de trabajo. Para mejorar la escalabilidad de la función de trabajo, puede escribir código multiproceso o usar código asincrónico para implementar [programación paralela](https://msdn.microsoft.com/library/ff963553.aspx). El ejemplo no implementa la programación en paralelo, pero muestra cómo convertir el código asincrónico, por lo que puede implementar la programación en paralelo.

## <a name="summary"></a>Resumen

En este capítulo, ha visto cómo mejorar la escalabilidad, confiabilidad y la capacidad de respuesta de la aplicación al implementar el patrón de trabajo centrado en colas.

Esta es la última de los 13 patrones que se tratan en este libro electrónico, pero por supuesto, hay muchos otros patrones y prácticas que pueden ayudarle a creación aplicaciones de la correcta en la nube. El [capítulo final](more-patterns-and-guidance.md) proporciona vínculos a recursos de temas que aún no se han tratado en estos 13 patrones.

<a id="resources"></a>
## <a name="resources"></a>Recursos

Para obtener más información acerca de las colas, consulte los siguientes recursos.

Documentación:

- [Microsoft Azure Storage colas, parte 1: Introducción a](http://justazure.com/microsoft-azure-storage-queues-part-1-getting-started/). Artículo de Schacherl romano.
- [Ejecutar tareas en segundo plano](https://msdn.microsoft.com/library/ff803365.aspx), capítulo 5 de [mover aplicaciones a la nube, 3ª edición](https://msdn.microsoft.com/library/ff728592.aspx) desde Microsoft Patterns and Practices. (En concreto, la sección ["Uso de colas de Azure Storage"](https://msdn.microsoft.com/library/ff803365.aspx#sec7).)
- [Procedimientos recomendados para maximizar la escalabilidad y rentabilidad de las soluciones de mensajería basadas en cola en Azure](https://msdn.microsoft.com/library/windowsazure/hh697709.aspx). Notas del producto por Valery Mizonov.
- [Comparación de las colas de Azure y colas de Service Bus](https://msdn.microsoft.com/magazine/jj159884.aspx). Artículo de MSDN Magazine, proporciona información adicional que puede ayudarle a elegir qué servicio de cola que se usará. El artículo menciona que Service Bus depende ACS para la autenticación, lo que significa que las colas de SB no estarán disponibles cuando ACS no está disponible. Sin embargo, puesto que se escribió el artículo, SB se cambió para que pueda usar [tokens de SAS](https://msdn.microsoft.com/library/windowsazure/dn170477.aspx) como una alternativa a ACS.
- [Microsoft Patterns and Practices - Guía de Azure](https://msdn.microsoft.com/library/dn568099.aspx). Consulte el manual de mensajería asincrónica, patrón Pipes and Filters, patrón Compensating Transaction, el patrón de consumidores en competencia, el patrón CQRS.
- [CQRS Journey](https://msdn.microsoft.com/library/jj554200). Libro electrónico sobre CQRS Microsoft Patterns and Practices.

Vídeo:

- [FailSafe: Creación de servicios de nube escalables, resistentes](https://channel9.msdn.com/Series/FailSafe). Serie de vídeos de nueve partes, Ulrich Homann, Marc Mercuri y Mark Simms. Presenta conceptos y principios de la arquitectura de una manera muy accesible e interesante, con casos procedentes de la experiencia del equipo de asesoramiento al cliente (CAT) de Microsoft con clientes reales. Para obtener una introducción al servicio de almacenamiento de Azure y colas, consulte episodio 5 comenzando en 35:13.

> [!div class="step-by-step"]
> [Anterior](distributed-caching.md)
> [Siguiente](more-patterns-and-guidance.md)
