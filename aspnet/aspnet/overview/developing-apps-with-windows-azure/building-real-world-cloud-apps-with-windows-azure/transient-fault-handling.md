---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/transient-fault-handling
title: "Transient Fault Handling (creación de aplicaciones de nube reales con Azure) | Documentos de Microsoft"
author: MikeWasson
description: "Las aplicaciones de nube de creación Real World con libros electrónicos Azure se basa en una presentación desarrollada por Scott Guthrie. Se explican 13 patrones y prácticas recomendadas que puede..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/03/2015
ms.topic: article
ms.assetid: 7ead83bc-c08c-4b26-8617-00e07292e35c
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/transient-fault-handling
msc.type: authoredcontent
ms.openlocfilehash: 3caeeb83e4c074ae0ffc30f035d793a821eb6be2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="transient-fault-handling-building-real-world-cloud-apps-with-azure"></a>Transient Fault Handling (creación de aplicaciones de nube reales con Azure)
====================
por [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Descarga corregirlo proyecto](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) o [descargar libros electrónicos](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> El **compilar aplicaciones de nube de mundo Real con Azure** libros electrónicos se basa en una presentación desarrollada por Scott Guthrie. Se explican los 13 patrones y prácticas recomendadas que le permitirá tener éxito el desarrollo de aplicaciones web para la nube. Para obtener información sobre el libro electrónico, consulte [el primer capítulo](introduction.md).


Cuando diseña una aplicación de nube del mundo real, una de las acciones que tienen que preocuparse es cómo controlar las interrupciones de servicio temporal. Este problema es importante de forma única en las aplicaciones de nube porque está por lo que depende de las conexiones de red y los servicios externos. Con frecuencia puede obtener poco problemas técnicos que normalmente se recuperación automática y, si no está preparado para controlarlos de forma inteligente, podrá dar lugar a una mala experiencia para sus clientes.

## <a name="causes-of-transient-failures"></a>Causas de errores transitorios

En el entorno de nube, encontrará que no se pudo y coloca las conexiones de base de datos que aparezcan periódicamente. Que es en parte porque se vayan a través de equilibradores de carga más en comparación con el entorno local en el servidor web y el servidor de base de datos tienen una conexión física directa. Además, en ocasiones, cuando esté depende de un servicio multiempresa verá llamadas al servicio get más lento o tiempo de espera porque otra persona, que utiliza el servicio está alcanzando mucho. En otros casos es posible que el usuario que está alcanzando el servicio con demasiada frecuencia, y el servicio se – deniega las conexiones: limita deliberadamente con el fin de evitar que afecte negativamente a otros inquilinos del servicio.

## <a name="use-smart-retryback-off-logic-to-mitigate-the-effect-of-transient-failures"></a>Use lógica de reintento/interrupción inteligente para mitigar el efecto de los errores transitorios

En lugar de producir una excepción y mostrar una página de error o no está disponible para su cliente, puede reconocer los errores que suelen ser temporales y automáticamente e intente la operación que generó el error, en espera que antes de tiempo que sea correcta. La mayoría del tiempo de la operación se realizará correctamente en el segundo intento y podrá recuperarse del error sin que el cliente alguna vez tenga en cuenta que ha habido un problema con.

Hay varias maneras de implementar lógica de reintento inteligentes.

- Microsoft Patterns &amp; prácticas grupo tiene un [Transient Fault Handling Application Block](https://msdn.microsoft.com/en-us/library/dn440719(v=pandp.60).aspx) que hace todo lo si usa ADO.NET para el acceso a la base de datos SQL (no a través de Entity Framework). Solo tiene que establecer una directiva de reintentos: el número de veces para volver a intentar una consulta o comando y cuánto tiempo debe esperar entre intentos: y ajuste su SQL código en un *con* bloque.

    [!code-csharp[Main](transient-fault-handling/samples/sample1.cs)]

    También admite TFH [caché en rol de Azure](https://msdn.microsoft.com/en-us/library/windowsazure/dn386103.aspx) y [Bus de servicio](https://azure.microsoft.com/services/service-bus/).
- Cuando use Entity Framework que normalmente no está trabajando directamente con las conexiones de SQL, por lo que no se puede usar este paquete de modelos y prácticas, pero Entity Framework 6 crea este tipo de lógica de reintento directamente en el marco de trabajo. De forma similar especifica la estrategia de reintento y, a continuación, usa dicha estrategia en EF cuando obtiene acceso a la base de datos.

    Para usar esta característica en la aplicación repararlo, todo lo que queda por hacer es agregar una clase que deriva de *DbConfiguration* y activar la lógica de reintento.

    [!code-csharp[Main](transient-fault-handling/samples/sample2.cs)]

    Para las excepciones de base de datos SQL que identifica el marco de trabajo como los errores transitorios por lo general, el código que se muestra indica EF para volver a intentar la operación hasta 3 veces, con un retraso de retroceso exponencial entre reintentos y un retraso máximo de 5 segundos. Retroceso exponencial significa que después de cada reintento errores va a esperar un período más largo de tiempo antes de volver a intentarlo. Si se produce un error en tres intentos de una fila, producirá una excepción. La siguiente sección sobre los disyuntores explica por qué desea retroceso exponencial y un número limitado de reintentos.

    Puede tener problemas similares cuando se usa el servicio de almacenamiento de Azure, tal y como hace la aplicación repararlo para Blobs y la API de cliente de almacenamiento de .NET ya implementa el mismo tipo de lógica. Solo debe especificar la directiva de reintentos, o incluso no tiene que hacerlo si está satisfecho con la configuración predeterminada.

<a id="circuitbreakers"></a>
## <a name="circuit-breakers"></a>Disyuntores

Hay varias razones por las que no para volver a intentar demasiadas veces durante un período demasiado largo:

- Demasiados usuarios reintentar las solicitudes con error de forma persistente pueden degradar la experiencia de otros usuarios. Si hay millones de personas todos lo repetidas solicitudes de reintento podría ocupar las colas de envío IIS e impide que atiendan las solicitudes que de lo contrario pudiera controlar correctamente la aplicación.
- Si todos los usuarios está reintentando debido a un error de servicio, se podrían colocar tantas solicitudes en cola el servicio obtiene congestiona al empezar a recuperar.
- Si el error es debido a la limitación y hay un intervalo de tiempo que el servicio utiliza para la limitación, continuadas reintentos pudieron mover esa ventana y hacer que la limitación de peticiones continuar.
- Es posible que tenga un usuario que se espera para una página web para representar. Hacer que personas espera demasiado larga podría molestar más esa relativamente rápidamente y les alerta para intentarlo de nuevo más tarde.

Retroceso exponencial trata algunos de estos problema mediante la limitación de la frecuencia de reintentos que puede obtener un servicio de la aplicación. Pero también necesita tener *los disyuntores*: Esto significa que en un determinado vuelva a intentar umbral de la aplicación deja de intentarlo y realiza otra acción, como uno de los siguientes:

- Retroceso personalizado. Si no se puede obtener un precio de las acciones de Reuters, quizá puede obtenerlo de Bloomberg; o bien, si no se puede obtener datos de la base de datos, quizá puede obtenerlo de caché.
- Producirá un error silencioso. Si lo que necesita de un servicio no está todo o nada de la aplicación, simplemente devolver null cuando no se puede obtener los datos. Si se muestra una tarea repararlo y el servicio no responde, podría mostrar los detalles de tareas sin la imagen.
- Error rápido. Error del usuario para evitar inundar el servicio con solicitudes que pudieron provocar la interrupción del servicio para otros usuarios o ampliar un período de limitación de reintento. Puede mostrar un mensaje descriptivo "inténtelo de nuevo más tarde".

No hay ninguna directiva de reintentos única. Puede reintentar más veces y esperar más en un proceso de trabajo asincrónico en segundo plano que lo haría en una aplicación web sincrónico donde un usuario está esperando una respuesta. Puede esperar más entre los reintentos para un servicio de base de datos relacional que lo haría para un servicio de caché. Estos son algunos ejemplos recomienda directivas para proporcionarle una idea de cómo pueden variar los números de reintento. (Ningún retraso antes del primer reintento significa "Rápido primero".

![Directivas de reintento de ejemplo](transient-fault-handling/_static/image1.png)

Para obtener instrucciones de directiva de reintento de base de datos SQL, consulte [solucionar los errores de conexión a base de datos de SQL y los errores transitorios](https://azure.microsoft.com/documentation/articles/sql-database-connectivity-issues/).

## <a name="summary"></a>Resumen

Una estrategia de reintento/interrupción puede ayudar a hacer errores temporales sea invisible para el cliente la mayoría de las veces, y Microsoft proporciona marcos de trabajo que puede usar para minimizar el trabajo de implementación de una estrategia si está utilizando ADO.NET, Entity Framework o Azure Servicio de almacenamiento.

En el [siguiente capítulo](distributed-caching.md), analizaremos cómo mejorar el rendimiento y confiabilidad mediante el uso de caching distribuido.

## <a name="resources"></a>Recursos

Para obtener más información, vea los siguientes recursos:

Documentación

- [Procedimientos recomendados para el diseño de servicios a gran escala en los servicios de nube de Azure](https://msdn.microsoft.com/en-us/library/windowsazure/jj717232.aspx). Notas del producto, Mark Simms y Michael Thomassy. Similar a la serie Failsafe pero queda procedimientos con mayor detalle. Vea la sección telemetría y diagnósticos.
- [Failsafe: Instrucciones para crear arquitecturas de nube resistentes](https://msdn.microsoft.com/en-us/library/windowsazure/jj853352.aspx). Notas del producto Marc Mercuri, Ulrich Homann y Andrew Townhill. Versión de la página Web de la serie de vídeos de FailSafe.
- [Microsoft patrones y prácticas - Guía de Azure](https://msdn.microsoft.com/en-us/library/dn568099.aspx). Vea reintento patrón, el patrón de Supervisor de agente de programador.
- [La tolerancia a errores en la base de datos SQL Azure](https://blogs.msdn.com/b/windowsazure/archive/2012/07/30/fault-tolerance-in-windows-azure-sql-database.aspx). Blog de Tony Petrossian.
- [Entity Framework - resistencia de conexión / lógica de reintento](https://msdn.microsoft.com/en-us/data/dn456835). Cómo usar y personalizar la característica de Entity Framework 6 de control de errores transitorios.
- [Resistencia de conexión y comando interceptación con Entity Framework en una aplicación ASP.NET MVC](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md). Cuarto en una serie de tutoriales de nueve partes, se muestra cómo configurar la característica de resistencia de conexión de EF 6 para la base de datos SQL.

Vídeos

- [FailSafe: Creación de servicios en la nube escalables y resistentes](https://channel9.msdn.com/Series/FailSafe). Serie de nueve partes por Ulrich Homann y Marc Mercuri, Mark Simms. Presenta los conceptos y principios de la arquitectura de una manera muy interesante y accesible, con casos extraídos de la experiencia del equipo de asesoramiento al cliente (CAT) de Microsoft con clientes reales. Vea la explicación de los disyuntores episodio 3 empieza en 40:55.
- [Creación Big: Lecciones aprendidas de clientes de Azure, parte II](https://channel9.msdn.com/Events/Build/2012/3-030). Mark Simms habla sobre el diseño para error transitorio de error de control y la instrumentación todo el contenido.

Ejemplo de código

- [Fundamentos de servicio en Azure en la nube](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649). Ejemplo de aplicación creada por el equipo de asesoramiento de cliente Azure de Microsoft que se muestra cómo utilizar el [Enterprise Library Transient Fault Handling Block](http://nuget.org/packages/EnterpriseLibrary.TransientFaultHandling/) (TFH). Para obtener más información, consulte [nube servicio Fundamentos capa acceso a datos: administración de errores transitorios](https://social.technet.microsoft.com/wiki/contents/articles/18665.cloud-service-fundamentals-data-access-layer-transient-fault-handling.aspx). TFH se recomienda para el acceso de la base de datos con ADO.NET directamente (sin usar Entity Framework).

>[!div class="step-by-step"]
[Anterior](monitoring-and-telemetry.md)
[Siguiente](distributed-caching.md)
