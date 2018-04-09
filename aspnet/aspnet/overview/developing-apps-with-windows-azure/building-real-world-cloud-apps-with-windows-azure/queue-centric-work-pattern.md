---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern
title: Patrón de trabajo centrado en cola (creación de aplicaciones de nube reales con Azure) | Documentos de Microsoft
author: MikeWasson
description: Las aplicaciones de nube de creación Real World con libros electrónicos Azure se basa en una presentación desarrollada por Scott Guthrie. Se explican 13 patrones y prácticas recomendadas que puede...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: cc1ad51b-40c3-4c68-8620-9aaa0fd1f6cf
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern
msc.type: authoredcontent
ms.openlocfilehash: 124e673206ecea2eac5efb8c2802a32a690fa104
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="queue-centric-work-pattern-building-real-world-cloud-apps-with-azure"></a>Patrón de trabajo centrado en cola (creación de aplicaciones de nube reales con Azure)
====================
por [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Descarga corregirlo proyecto](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) o [descargar libros electrónicos](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> El **compilar aplicaciones de nube de mundo Real con Azure** libros electrónicos se basa en una presentación desarrollada por Scott Guthrie. Se explican los 13 patrones y prácticas recomendadas que le permitirá tener éxito el desarrollo de aplicaciones web para la nube. Para obtener información sobre el libro electrónico, consulte [el primer capítulo](introduction.md).


Anteriormente, hemos visto que el uso de varios servicios puede producir en un SLA "compuesto", donde es el SLA efectivo de la aplicación la *producto* de los SLA individuales. Por ejemplo, la aplicación repararlo utiliza sitios Web, el almacenamiento y la base de datos SQL. Si se produce un error en cualquiera de estos servicios, la aplicación devolverá un error al usuario.

Almacenamiento en caché es una buena forma de controlar los errores transitorios para el contenido de solo lectura. Pero ¿qué ocurre si la aplicación necesita realizar el trabajo? Por ejemplo, cuando el usuario envía una nueva repararlo tarea, la aplicación simplemente no puede poner la tarea en la memoria caché. Debe escribir la tarea repararlo en un almacén de datos persistente, por lo que se puede procesar la aplicación.

Que es donde entra en juego el patrón centrada en la cola de trabajo. Este modelo permite acoplamiento flexible entre un nivel de web y un servicio back-end.

Le mostramos cómo funciona el patrón. Cuando la aplicación recibe una solicitud, se coloca un elemento de trabajo en una cola y devuelve inmediatamente la respuesta. A continuación, un proceso independiente de back-end extrae los elementos de trabajo de la cola y realiza el trabajo.

El patrón de trabajo centrado en la cola es útil para:

- Trabajo que requiere mucho tiempo (latencia alta).
- Trabajo que necesita un servicio externo que no siempre estén disponible.
- El trabajo que se requieren muchos recursos (CPU alta).
- Trabajo que se beneficiaría de velocidad redistribuir (sujeto a ráfagas de carga repentina).

## <a name="reduced-latency"></a>Reducir la latencia

Las colas son útiles en cualquier momento que se está realizando trabajo que consumen muchos recursos. Si una tarea tarda unos segundos o más, en lugar de bloquear al usuario final, coloca el elemento de trabajo en una cola. Indicar al usuario "Estamos trabajando en él" y, a continuación, usar un agente de escucha de cola para procesar la tarea en segundo plano.

Por ejemplo, cuando se realiza una compra un comerciante en línea, el sitio web confirma su pedido inmediatamente. Pero eso no significa que el material ya está en un camión entregando. Coloca una tarea en una cola, y en segundo plano está realizando la comprobación de crédito, preparación de los elementos para su envío y así sucesivamente.

Para escenarios con latencia cortos, el tiempo total de-to-end puede ser mayor mediante una cola, en comparación con realizar la tarea de forma sincrónica. Pero aún así, las otras ventajas pueden sobrepasar ese inconveniente.

## <a name="increased-reliability"></a>Aumentar la confiabilidad

En la versión de repararlo que nos hemos visto hasta ahora, el front-end web se acopla estrechamente con la base de datos SQL back-end. Si el servicio de base de datos SQL no está disponible, el usuario obtiene un error. Si los reintentos no funcionan (es decir, el error transitorio más de), lo único que puede hacer es mostrar un error y pedir al usuario que intente de nuevo más tarde.

![Diagrama que muestra servidor web cliente errores cuando se produce un error en la base de datos SQL back-end](queue-centric-work-pattern/_static/image1.png)

Uso de colas, cuando un usuario envía una tarea repararlo, la aplicación escribe un mensaje a la cola. La carga del mensaje es una [JSON](http://json.org/) representación de la tarea. En cuanto el mensaje se escribe en la cola, la aplicación se devuelve y se muestra inmediatamente un mensaje de confirmación al usuario.

Si cualquiera de los servicios back-end: por ejemplo, la base de datos SQL o el agente de escucha de cola--queden sin conexión, los usuarios todavía pueden enviar nuevas repararlo tareas. Los mensajes sólo se pondrán en cola hasta que vuelven a están disponibles los servicios back-end. En ese momento, los servicios back-end se ponga al día en el trabajo pendiente.

![Diagrama que muestra web front-end continuar funcionando cuando se produce un error de base de datos SQL](queue-centric-work-pattern/_static/image2.png)

Además, ahora puede agregar más lógica de back-end sin preocuparse por la resistencia de front-end. Por ejemplo, puede enviar un correo electrónico o un mensaje SMS al propietario cada vez que se asigna un nuevo repararlo. Si el correo electrónico o el servicio SMS deja de estar disponible, puede todo lo demás procesar y, a continuación, se coloca un mensaje en una cola independiente para enviar mensajes de correo electrónico y SMS.

Anteriormente, nuestro acuerdo eficaz era aplicaciones Web &times; almacenamiento &times; base de datos de SQL = 99,7%. (Consulte [diseño sobrevivir a fallas](design-to-survive-failures.md).)

Cuando cambiamos la aplicación para utilizar una cola, el front-end web depende solo de las aplicaciones Web y almacenamiento, para un SLA de 99,8% compuesto. (Tenga en cuenta que las colas son parte del servicio de almacenamiento de Azure, por lo que se incluyen en el mismo SLA como almacenamiento de blobs).

Si necesita incluso mejor que 99,8%, puede crear dos colas en dos regiones diferentes. Designar uno como principal y el otro como la base de datos secundaria. En la aplicación, conmutación por error a la cola secundaria si la cola principal no está disponible. La posibilidad de que ambos estén disponibles al mismo tiempo es muy pequeña.

## <a name="rate-leveling-and-independent-scaling"></a>Redistribución de velocidad y escalamiento independiente

Las colas también son útiles para lo que se denomina *velocidad redistribución* o *nivelado de carga*.

Las aplicaciones Web a menudo son susceptibles a picos repentinos de tráfico. Aunque puede usar el escalado automático para agregar automáticamente servidores web para administrar el tráfico web mayor, escalado automático no pueda reaccionar rápidamente para atender los picos de carga abruptos. Si los servidores web pueden descargar algunas de las tareas que tienen que hacer mediante la escritura de un mensaje a una cola, puede administrar más tráfico. Un servicio back-end, a continuación, puede leer los mensajes de la cola y procesarlos. La profundidad de la cola se aumentar o reducir a medida que varía la carga entrante.

Con la mayor parte de su trabajo que consumen muchos recursos que se descargan a un servicio back-end, el nivel de web más fácilmente puede responder a picos repentinos en tráfico. Acciones y ahorrar dinero, porque cualquier transcurrido cierto tráfico pueda ser controlada por menos servidores web.

Puede escalar el nivel de web y el servicio de back-end por separado. Por ejemplo, tendrá que tres servidores web pero solo mensajes de cola de procesamiento de servidor. O bien, si está ejecutando una tarea de proceso intensivo en segundo plano, puede que tenga más servidores de back-end.

![](queue-centric-work-pattern/_static/image3.png)

Escala automática funciona con servicios back-end, así como con el nivel de web. Puede aumentar o reducir el número de máquinas virtuales que se están procesando las tareas en la cola, según el uso de CPU del máquinas virtuales de back-end. O bien, puede que el escalado automático en función de cuántos elementos en una cola. Por ejemplo, puede indicar el escalado automático para intentar mantener no más de 10 elementos en la cola. Si la cola tiene más de 10 elementos, escalado automático agregará las máquinas virtuales. Cuando ponerse al día, escalado automático hará caer las máquinas virtuales adicionales.

## <a name="adding-queues-to-the-fix-it-application"></a>Agregarlo pone en cola para la corrección de aplicación

Para implementar el patrón de cola, debemos hacer dos cambios en la aplicación repararlo.

- Cuando un usuario envía una nueva repararlo tarea, coloque la tarea en la cola, en lugar de escribir en la base de datos.
- Crear un servicio back-end que procesa los mensajes en la cola.

En la cola, se utilizará el [servicio de almacenamiento de cola de Azure](https://www.windowsazure.com/develop/net/how-to-guides/queue-service/). Otra opción consiste en usar [Service Bus de Azure](https://docs.microsoft.com/azure/service-bus/).

Para decidir qué servicio de cola que se usará, considere la forma en que la aplicación necesita para enviar y recibir los mensajes en la cola:

- Si tienes cooperativos productores y consumidores en competencia, considere el uso de servicio de almacenamiento de cola de Azure. "Productores cooperar" significa que varios procesos son agregar mensajes a una cola. "Consumidores en competencia" significa que varios procesos están extrayendo los mensajes de la cola para procesarlos, pero un mensaje determinado solo puede ser procesado por un "consumidor". Si necesita más rendimiento del que se puede obtener con una sola cola, usar colas adicionales o cuentas de almacenamiento adicional.
- Si necesita un [modelo de publicación/suscripción](http://en.wikipedia.org/wiki/Publish/subscribe), considere el uso de colas de Bus de servicio de Azure.

La aplicación repararlo se adapta a los productores cooperativos y competencia modelo de los consumidores.

Otra consideración es la disponibilidad de las aplicaciones. El servicio de almacenamiento de cola es parte del mismo servicio que usamos para el almacenamiento de blobs, para que usarla no tiene ningún efecto en el SLA. Service Bus de Azure es un servicio independiente con su propio SLA. Si se utilizan colas de Bus de servicio, se tendría que tener en cuenta un porcentaje del SLA adicional y nuestro SLA compuesto sería inferior. Elegir un servicio de cola, asegúrese de que conoce el impacto de su elección en la disponibilidad de las aplicaciones. Para obtener más información, consulte el [recursos](#resources) sección.

## <a name="creating-queue-messages"></a>Creación de cola de mensajes

Para colocar una tarea repararlo en la cola, el front-end web realiza los pasos siguientes:

1. Crear un [CloudQueueClient](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueueclient.aspx) instancia. El `CloudQueueClient` instancia se utiliza para ejecutar solicitudes en el servicio de cola.
2. Crear la cola, si no existe todavía.
3. Serializar la tarea repararlo.
4. Llame a [CloudQueue.AddMessageAsync](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueue.addmessageasync.aspx) para colocar el mensaje en la cola.

Se deberá llevar a cabo este trabajo en el constructor y `SendMessageAsync` método de un nuevo `FixItQueueManager` clase.

[!code-csharp[Main](queue-centric-work-pattern/samples/sample1.cs?highlight=11-12,16,18-25)]

Aquí usamos la [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) library para serializar el fixit en formato JSON. Puede usar cualquier método de serialización que prefiera. JSON tiene la ventaja de ser legible, pero menos detallados que XML.

Código de calidad de producción debería agregar lógica de control de errores, pausar si la base de datos deja de estar disponible, controlar más limpiamente recuperación, crear la cola en el inicio de la aplicación y administrar "[dudosos" mensajes](https://msdn.microsoft.com/library/ms789028(v=vs.110).aspx). (Un mensaje dudoso es un mensaje que no se puede procesar por algún motivo. No desea que los mensajes dudosos que se colocan en la cola, donde el rol de trabajo continuamente intentará procesarlos, producirá un error, vuelva a intentarlo, producirá un error y así sucesivamente.)

En la aplicación de MVC front-end, deberá actualizar el código que crea una nueva tarea. En lugar de colocar la tarea en el repositorio, llame a la `SendMessageAsync` método mostrado anteriormente.

[!code-csharp[Main](queue-centric-work-pattern/samples/sample2.cs?highlight=10)]

## <a name="processing-queue-messages"></a>Mensajes de la cola de procesamiento

Para procesar mensajes de la cola, vamos a crear un servicio back-end. El servicio back-end ejecutará un bucle infinito que lleva a cabo los pasos siguientes:

1. Obtener el siguiente mensaje de la cola.
2. Deserializar el mensaje a una tarea repararlo.
3. Escribir la tarea repararlo en la base de datos.

Para hospedar el servicio back-end, vamos a crear un servicio de nube de Azure que contiene un *rol de trabajo*. Un rol de trabajo consta de uno o más máquinas virtuales que pueden realizar el procesamiento de back-end. El código que se ejecuta en estas máquinas virtuales extraerá los mensajes de la cola en cuanto estén disponibles. Para cada mensaje, comenzaremos deserializar la carga de JSON y escribir una instancia de la entidad de corregir su tarea a la base de datos con el mismo repositorio que hemos usado anteriormente en el nivel de web.

Los pasos siguientes muestran cómo agregar un trabajo de proyecto de rol para una solución que tenga un proyecto web estándar. Estos pasos ya se han realizado en el proyecto repararlo que puede descargar.

En primer lugar, agregue un proyecto de servicio de nube a la solución de Visual Studio. Haga clic en la solución y seleccione **agregar**, a continuación, **nuevo proyecto**. En el panel izquierdo, expanda **Visual C#** y seleccione **nube**.

[![](queue-centric-work-pattern/_static/image5.png)](queue-centric-work-pattern/_static/image4.png)

En el **nuevo servicio de nube de Azure** cuadro de diálogo, expanda el **Visual C#** nodo en el panel izquierdo. Seleccione **rol de trabajo** y haga clic en el icono de flecha derecha.

![](queue-centric-work-pattern/_static/image6.png)

(Tenga en cuenta que también puede agregar un *rol web*. Podríamos ejecutar la repararlo front-end en el mismo servicio de nube en lugar de ejecutarse en un sitio Web de Azure. Que tiene algunas ventajas en lo facilita coordinar las conexiones entre los front-end y back-end. Sin embargo, para simplificar esta demostración, estamos teniendo el front-end en una aplicación de Web de servicio de aplicación de Azure y solo se ejecuta el back-end en un servicio de nube.)

Se asigna un nombre predeterminado para el rol de trabajo. Para cambiar el nombre, mantenga el mouse sobre el rol de trabajo en el panel derecho y, a continuación, haga clic en el icono de lápiz.

![](queue-centric-work-pattern/_static/image7.png)

Haga clic en **Aceptar** para completar el cuadro de diálogo. Esto agrega dos proyectos a la solución de Visual Studio.

- un proyecto de Azure que define el servicio de nube, incluida la información de configuración.
- Un proyecto de rol de trabajo que define el rol de trabajo.

![](queue-centric-work-pattern/_static/image8.png)

Para obtener más información, vea [crear un proyecto de Azure con Visual Studio.](https://msdn.microsoft.com/library/windowsazure/ee405487.aspx)

Dentro de la función de trabajo, se sondear en busca de mensajes mediante una llamada a la `ProcessMessageAsync` método de la `FixItQueueManager` clase que hemos visto antes.

[!code-csharp[Main](queue-centric-work-pattern/samples/sample3.cs?highlight=25)]

El `ProcessMessagesAsync` método comprueba si hay un mensaje en espera. Si lo hay, deserializa el mensaje en una `FixItTask` entidad y guarda la entidad en la base de datos. Repite hasta que la cola está vacía.

[!code-csharp[Main](queue-centric-work-pattern/samples/sample4.cs)]

Sondear la cola de mensajes incurre en una transacción pequeña cobran, por lo que cuando no hay ningún mensaje que esperan ser procesados, el rol de trabajo `RunAsync` método espera un segundo antes de sondeo de nuevo mediante una llamada a `Task.Delay(1000)`.

En un proyecto web, agregar código asincrónico puede automáticamente mejorar el rendimiento porque IIS administra un grupo de subprocesos limitado. No es el caso de un proyecto de rol de trabajo. Para mejorar la escalabilidad de la función de trabajo, puede escribir código multiproceso o usar código asincrónico para implementar [la programación paralela](https://msdn.microsoft.com/library/ff963553.aspx). El ejemplo no implementa la programación en paralelo, pero muestra cómo convertir el código asincrónico, por lo que puede implementar la programación paralela.

## <a name="summary"></a>Resumen

En este capítulo ha visto cómo mejorar la escalabilidad, confiabilidad y capacidad de respuesta de aplicación al implementar el patrón de trabajo centrado en la cola.

Esta es la última de las 13 patrones descritos en este libro electrónico, pero por supuesto, existen muchos otros patrones y prácticas recomendadas que pueden ayudarle a crear aplicaciones de nube adecuada. El [capítulo final](more-patterns-and-guidance.md) proporciona vínculos a recursos de temas en los que aún no se hayan tratado en estos 13 patrones.

<a id="resources"></a>
## <a name="resources"></a>Recursos

Para obtener más información acerca de las colas, vea los siguientes recursos.

Documentación:

- [Parte de las colas de almacenamiento de Microsoft Azure 1: Introducción a](http://justazure.com/microsoft-azure-storage-queues-part-1-getting-started/). Artículo por Schacherl latino.
- [Ejecutar tareas en segundo plano](https://msdn.microsoft.com/library/ff803365.aspx), capítulo 5 de [mover aplicaciones a la nube, 3ª edición](https://msdn.microsoft.com/library/ff728592.aspx) desde Microsoft Patterns and Practices. (En concreto, la sección ["Uso de las colas de almacenamiento de Azure"](https://msdn.microsoft.com/library/ff803365.aspx#sec7).)
- [Procedimientos recomendados para maximizar la escalabilidad y la rentabilidad de las soluciones de mensajes basadas en cola en Azure](https://msdn.microsoft.com/library/windowsazure/hh697709.aspx). Notas del producto por Valery Mizonov.
- [Comparación de las colas de Azure y colas de Service Bus](https://msdn.microsoft.com/magazine/jj159884.aspx). Artículo de MSDN Magazine, proporciona información adicional que puede ayudarle a elegir qué servicio de cola que se usará. El artículo menciona que Service Bus es dependiente de ACS para la autenticación, lo que significa que las colas de SB podrían no estar disponibles cuando ACS no está disponible. Sin embargo, puesto que el artículo se escribió, SB se cambió para permitirle utilizar [tokens SAS](https://msdn.microsoft.com/library/windowsazure/dn170477.aspx) como una alternativa a ACS.
- [Microsoft patrones y prácticas - Guía de Azure](https://msdn.microsoft.com/library/dn568099.aspx). Consulte el manual de mensajería asincrónica, canalizaciones y filtros de modelo, patrón de transacciones de compensación, patrón de consumidores en competencia, patrón CQRS.
- [Viaje a CQRS](https://msdn.microsoft.com/library/jj554200). Libro electrónico sobre CQRS por Microsoft patrones y procedimientos recomendados.

Video:

- [FailSafe: Creación de servicios en la nube escalables y resistentes](https://channel9.msdn.com/Series/FailSafe). Serie de vídeos de nueve partes por Ulrich Homann y Marc Mercuri, Mark Simms. Presenta los conceptos y principios de la arquitectura de una manera muy interesante y accesible, con casos extraídos de la experiencia del equipo de asesoramiento al cliente (CAT) de Microsoft con clientes reales. Para obtener una introducción a las colas y el servicio de almacenamiento de Azure, vea episodio 5 a partir de 35:13.

> [!div class="step-by-step"]
> [Anterior](distributed-caching.md)
> [Siguiente](more-patterns-and-guidance.md)
