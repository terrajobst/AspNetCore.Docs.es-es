---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/transient-fault-handling
title: Transient Fault Handling (creación de aplicaciones de nube reales con Azure) | Microsoft Docs
author: MikeWasson
description: Building Real World Cloud Apps with e-book de Azure se basa en una presentación desarrollada por Scott Guthrie. Explican el 13 de patrones y prácticas que puede...
ms.author: aspnetcontent
ms.date: 11/03/2015
ms.assetid: 7ead83bc-c08c-4b26-8617-00e07292e35c
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/transient-fault-handling
msc.type: authoredcontent
ms.openlocfilehash: e67f3106f060d52f90ba56d6684af64779009e39
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37823383"
---
<a name="transient-fault-handling-building-real-world-cloud-apps-with-azure"></a>Transient Fault Handling (creación de aplicaciones de nube reales con Azure)
====================
por [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Descargar proyecto corregirlo](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) o [descargar libro electrónico](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> El **Building Real World Cloud Apps with Azure** eBook se basa en una presentación desarrollada por Scott Guthrie. Se explican el 13 patrones y prácticas que pueden ayudarle a tener éxito el desarrollo de aplicaciones web para la nube. Para obtener información sobre el libro electrónico, consulte [el primer capítulo](introduction.md).


Cuando diseña una aplicación de nube del mundo real, una de las cosas que debe considerar es cómo controlar las interrupciones de servicio temporal. Este problema es importante de forma exclusiva en aplicaciones en la nube porque se encuentra por lo que depende de las conexiones de red y los servicios externos. Puede obtener con frecuencia pequeños problemas normalmente Autorrecuperación y, si no está preparada para controlarlos de forma inteligente, generará una mala experiencia para sus clientes.

## <a name="causes-of-transient-failures"></a>Causas de errores transitorios

En el entorno de nube, encontrará que no se pudo y conexiones de base de datos eliminadas se producen periódicamente. Que es en parte porque vas a través de equilibradores de carga más en comparación con el entorno local donde el servidor web y el servidor de base de datos tienen una conexión física directa. Además, en ocasiones, cuando esté depende de un servicio multiempresa verá las llamadas al servicio se vuelven lentos o tiempo de espera porque otra persona, que usa el servicio esté alcanzando mucho. En otros casos puede que el usuario que se comunica con el servicio con demasiada frecuencia, y el servicio limita deliberadamente le: deniega las conexiones: con el fin de evitar que afecten negativamente a otros inquilinos del servicio.

## <a name="use-smart-retryback-off-logic-to-mitigate-the-effect-of-transient-failures"></a>Usar lógica de reintento/interrupción inteligente para mitigar el efecto de los errores transitorios

En lugar de producir una excepción y mostrar una página de error o no está disponible para su cliente, puede reconocer los errores que suelen ser temporales y automáticamente a intentar la operación que produjo el error, en espera que antes de cuánto tiempo estará correcta. La mayoría del tiempo de la operación se realizará correctamente en el segundo intento, y podrá recuperarse del error sin que el cliente nunca tener fuesen conscientes de que ha habido un problema.

Hay varias maneras de implementar lógica de reintento inteligentes.

- El Microsoft Patterns &amp; prácticas grupo tiene un [Transient Fault Handling Application Block](https://msdn.microsoft.com/library/dn440719(v=pandp.60).aspx) que hace todo automáticamente si usa ADO.NET para el acceso a la base de datos SQL (no a través de Entity Framework). Solo tiene que establecer una directiva de reintentos: número de veces para volver a intentar una consulta o comando y el tiempo de espera entre intentos: y ajuste su SQL de código en un *mediante* bloque.

    [!code-csharp[Main](transient-fault-handling/samples/sample1.cs)]

    También admite TFH [Azure in-Role Cache](https://msdn.microsoft.com/library/windowsazure/dn386103.aspx) y [Service Bus](https://azure.microsoft.com/services/service-bus/).
- Al usar Entity Framework que normalmente no está trabajando directamente con las conexiones de SQL, por lo que no se puede usar este paquete de patrones y prácticas, pero Entity Framework 6 se basa este tipo de lógica de reintento en el marco de trabajo. De forma similar especifique la estrategia de reintento y, a continuación, EF usa dicha estrategia cada vez que tiene acceso a la base de datos.

    Para usar esta característica en la aplicación Fix It, lo único que debemos hacer es agregar una clase que derive de *DbConfiguration* y activar la lógica de reintento.

    [!code-csharp[Main](transient-fault-handling/samples/sample2.cs)]

    Para las excepciones de base de datos SQL que identifica el marco de trabajo como errores transitorios por lo general, el código que se muestra indica a EF que vuelva a intentar la operación hasta 3 veces, con un retraso de retroceso exponencial entre reintentos y un retraso máximo de 5 segundos. Retroceso exponencial significa que después de cada intento con error esperará durante un periodo de tiempo antes de intentarlo de nuevo. Si se producen errores en tres intentos en una fila, producirá una excepción. La siguiente sección sobre los disyuntores explica por qué desea retroceso exponencial y un número limitado de reintentos.

    Cuando se usa el servicio de almacenamiento de Azure, como hace la aplicación Fix It para Blobs y la API de cliente de almacenamiento .NET ya implementa el mismo tipo de lógica puede tener problemas similares. Simplemente especifique la directiva de reintentos, o incluso no tiene que hacerlo si está satisfecho con la configuración predeterminada.

<a id="circuitbreakers"></a>
## <a name="circuit-breakers"></a>Disyuntores

Hay varias razones por qué no desea reintentar demasiadas veces durante un período demasiado largo:

- Demasiados usuarios de forma persistente reintentar las solicitudes con error podrían degradar la experiencia de otros usuarios. Si millones de personas están todas realizar repetidas solicitudes de reintento podría afectar a las colas de envío IIS e impide que atienden solicitudes que en caso contrario, podría controlar correctamente la aplicación.
- Si todo el mundo está reintentando la operación debido a un error de servicio, se podrían colocar tantas solicitudes en cola el servicio obtiene atestado cuando empieza a recuperar.
- Si el error es debido a la limitación y hay un período de tiempo que el servicio utiliza para la limitación, reintentos continuos podrían mover esa ventana y hacer que la limitación continuar.
- Es posible que tenga un usuario esperando una página web para procesar. Hacer que las personas espera demasiado largo podría más molestos esa forma relativamente rápida que avisa de que vuelva a intentarlo.

Retroceso exponencial trata algunos de estos problemas. limitando la frecuencia de reintentos que se puede obtener un servicio de la aplicación. Pero también necesita tener *disyuntores*: Esto significa que en una determinada Reintentar umbral de la aplicación deja de intentarlo y realiza otra acción, como uno de los siguientes:

- Retroceso personalizado. Si no se puede obtener una cotización de Reuters, quizás se puede obtener de Bloomberg; o bien, si no se puede obtener datos de la base de datos, quizás se puede obtener de caché.
- Un error silencioso. Si lo que necesita desde un servicio no es todo o nada de la aplicación, que sólo devuelva null cuando no se puede obtener los datos. Si se muestra una tarea Fix It y Blob service no responde, podría mostrar los detalles de tarea sin la imagen.
- Error de inmediato. Error del usuario para evitar el desbordamiento del servicio con vuelva a intentar las solicitudes que podrían provocar la interrupción del servicio para otros usuarios o ampliar un período de limitación. Puede mostrar un mensaje descriptivo "vuelva a intentarlo más tarde".

No hay ninguna directiva de reintentos única. Puede efectuar más reintentos y esperar más tiempo en un proceso de trabajo asincrónico en segundo plano a como lo haría en una aplicación web sincrónico, donde un usuario está esperando una respuesta. Puede esperar más tiempo entre reintentos para un servicio de base de datos relacional a como lo haría para un servicio de caché. Estos son algunos ejemplo recomienda directivas para darle una idea de cómo pueden variar los números de reintentos. ("Fast primero" no significa que ningún retraso antes del primer reintento.

![Ejemplos de directivas de reintento](transient-fault-handling/_static/image1.png)

Para obtener información de la directiva de reintento de base de datos SQL, consulte [solucionar problemas de errores transitorios y errores de conexión a SQL Database](https://azure.microsoft.com/documentation/articles/sql-database-connectivity-issues/).

## <a name="summary"></a>Resumen

Una estrategia de reintento/interrupción puede ayudar a ocultar errores temporales al cliente la mayoría del tiempo, y Microsoft proporciona marcos de trabajo que puede usar para minimizar el trabajo de implementación de una estrategia independientemente de si usa ADO.NET, Entity Framework o Azure Servicio de almacenamiento.

En el [siguiente capítulo](distributed-caching.md), veremos cómo mejorar el rendimiento y confiabilidad mediante el uso de almacenamiento en caché distribuido.

## <a name="resources"></a>Recursos

Para obtener más información, vea los siguientes recursos:

Documentación

- [Procedimientos recomendados para el diseño de servicios a gran escala en Azure Cloud Services](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). Notas del producto, Mark Simms y Michael Thomassy. Similar a la serie Failsafe pero entran en obtener más información sobre procedimientos. Consulte la sección de telemetría y diagnósticos.
- [Failsafe: Instrucciones para crear arquitecturas de nube resistentes](https://msdn.microsoft.com/library/windowsazure/jj853352.aspx). Notas del producto, Marc Mercuri, Ulrich Homann y Andrew Townhill. Versión de la serie de vídeos a prueba de errores de página Web.
- [Microsoft Patterns and Practices - Guía de Azure](https://msdn.microsoft.com/library/dn568099.aspx). Consulte reintento pattern, el patrón Scheduler Agent Supervisor.
- [Tolerancia a errores en Azure SQL Database](https://blogs.msdn.com/b/windowsazure/archive/2012/07/30/fault-tolerance-in-windows-azure-sql-database.aspx). Blog de Tony Petrossian.
- [Entity Framework: resistencia de conexión / lógica de reintento](https://msdn.microsoft.com/data/dn456835). Cómo usar y personalizar las características de Entity Framework 6 de control de errores transitorios.
- [Resistencia de conexión e intercepción de comandos con Entity Framework en una aplicación ASP.NET MVC](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md). En cuarto lugar en una serie de tutoriales de nueve partes, se muestra cómo configurar la característica de resistencia de conexión de EF 6 para SQL Database.

Vídeos

- [FailSafe: Creación de servicios de nube escalables, resistentes](https://channel9.msdn.com/Series/FailSafe). Serie de nueve partes, Ulrich Homann, Marc Mercuri y Mark Simms. Presenta conceptos y principios de la arquitectura de una manera muy accesible e interesante, con casos procedentes de la experiencia del equipo de asesoramiento al cliente (CAT) de Microsoft con clientes reales. Vea la explicación de los disyuntores, episodio 3 empieza en 40:55.
- [Creación Big: Lecciones aprendidas de los clientes de Azure: parte II](https://channel9.msdn.com/Events/Build/2012/3-030). Mark Simms habla sobre el diseño de transitorio, error del error de control y la instrumentación todo.

Ejemplo de código

- [En la nube Fundamentos de servicio en Azure](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649). Ejemplo de aplicación creada por el equipo de asesoramiento de cliente Azure de Microsoft que se muestra cómo utilizar el [Enterprise Library Transient Fault Handling Block](http://nuget.org/packages/EnterpriseLibrary.TransientFaultHandling/) (TFH). Para obtener más información, consulte [en la nube capa Fundamentos del servicio Data Access: control de errores transitorios](https://social.technet.microsoft.com/wiki/contents/articles/18665.cloud-service-fundamentals-data-access-layer-transient-fault-handling.aspx). Se recomienda TFH acceso a la base de datos mediante ADO.NET directamente (sin usar Entity Framework).

> [!div class="step-by-step"]
> [Anterior](monitoring-and-telemetry.md)
> [Siguiente](distributed-caching.md)
