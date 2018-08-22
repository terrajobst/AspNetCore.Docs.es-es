---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/design-to-survive-failures
title: Diseño para sobrevivir a errores (crear aplicaciones de nube reales con Azure) | Microsoft Docs
author: MikeWasson
description: Building Real World Cloud Apps with e-book de Azure se basa en una presentación desarrollada por Scott Guthrie. Explican el 13 de patrones y prácticas que puede...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: 364ce84e-5af8-4e08-afc9-75a512b01f84
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/design-to-survive-failures
msc.type: authoredcontent
ms.openlocfilehash: 1098892c621b8add624b18788076be2fac1cc158
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838491"
---
<a name="design-to-survive-failures-building-real-world-cloud-apps-with-azure"></a>Diseño para sobrevivir a errores (crear aplicaciones de nube reales con Azure)
====================
por [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Descargar proyecto corregirlo](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) o [descargar libro electrónico](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> El **Building Real World Cloud Apps with Azure** eBook se basa en una presentación desarrollada por Scott Guthrie. Se explican el 13 patrones y prácticas que pueden ayudarle a tener éxito el desarrollo de aplicaciones web para la nube. Para obtener información sobre el libro electrónico, consulte [el primer capítulo](introduction.md).


Una de las cosas que debe considerar al compilar cualquier tipo de aplicación, pero especialmente uno que se ejecutará en la nube donde una gran cantidad de personas lo va a usar, es cómo diseñar la aplicación para que pueda controlar los errores correctamente y seguirán ofreciendo valor tanto como es posible. Con el tiempo suficiente, las cosas van a algún problema en cualquier entorno o cualquier sistema de software. Cómo la aplicación trata esas situaciones determina cómo malestar obtendrán los clientes y cuánto tiempo se debe analizar y corregir problemas.

## <a name="types-of-failures"></a>Tipos de errores

Hay dos categorías básicas de los errores que desea administrar de manera diferente:

- Transitorio, la recuperación automática errores tales como problemas de conectividad de red intermitentes.
- Soportar los errores que requieren intervención.

Para errores transitorios, puede implementar una directiva de reintentos para asegurarse de que la mayoría de las veces que se recupera de la aplicación de forma rápida y automática. Los clientes es posible que observe un poco más largo tiempo de respuesta, pero en caso contrario no se verán afectados. Le mostraremos algunas formas de controlar estos errores en el [capítulo de control de errores transitorios](transient-fault-handling.md).

Puede implementar para soportar errores, supervisión y la funcionalidad que le notifica inmediatamente cuando surgen problemas y que facilita el análisis de causa raíz del registro. Le mostraremos algunas maneras de ayudarle a mantenerse encima de estos tipos de errores en el [capítulo supervisión y telemetría](monitoring-and-telemetry.md).

## <a name="failure-scope"></a>Ámbito del error

También tiene que pensar en el ámbito de error: si se ve afectado un único equipo, un servicio completo como base de datos SQL, almacenamiento o toda una región.

![Ámbito del error](design-to-survive-failures/_static/image1.png)

### <a name="machine-failures"></a>Errores de equipo

En Azure, un servidor con errores se reemplaza automáticamente por una nueva y una aplicación de nube bien diseñada se recupera de este tipo de error de forma rápida y automática. Anteriormente se fuerzan las ventajas de escalabilidad de un nivel web sin estado y la facilidad de recuperación desde un servidor con errores es otra ventaja de no disponibilidad de Estados. Facilidad de recuperación también es una de las ventajas de las características de plataforma como servicio (PaaS) como SQL Database y Azure App Service Web Apps. Errores de hardware son poco frecuentes, pero cuando se producen que estos servicios controlan automáticamente; aún no tiene que escribir código para controlar los errores de la máquina cuando se usa uno de estos servicios.

### <a name="service-failures"></a>Errores de servicio

Aplicaciones en la nube suelen utilizan varios servicios. Por ejemplo, la aplicación Fix It utiliza el servicio de base de datos SQL, el servicio de almacenamiento y la aplicación web se implementa en Azure App Service. ¿Lo que la aplicación hará si se produce un error en uno de los servicios que depende de? Para algunos errores del servicio descriptivo "lo siento, inténtelo de nuevo más tarde" mensaje podría ser la mejor que puede hacer. Pero en muchos escenarios pueden hacerlo mejor. Por ejemplo, cuando el almacén de datos de back-end está inactivo, puede aceptar entradas de usuario, mostrar "se ha recibido la solicitud" y almacenar temporalmente; la entrada enteraba else a continuación, cuando el servicio que necesita está operativo de nuevo, puede recuperar la entrada y procesarla.

El [patrón centrado en colas de trabajo](queue-centric-work-pattern.md) capítulo muestra una forma de controlar este escenario. La aplicación Fix It almacena las tareas en la base de datos SQL, pero no tiene que salir de funcionar cuando la base de datos SQL está inactivo. En este capítulo, veremos cómo almacenar la entrada del usuario para una tarea en una cola y usar un proceso de trabajo para leer la cola y actualizar la tarea. Si SQL no funciona, la capacidad para crear tareas Fix It se ve afectada; el proceso de trabajo puede esperar y procesar tareas nuevas cuando la base de datos SQL está disponible.

### <a name="region-failures"></a>Errores de región

Regiones enteras pueden producir errores. Un desastre natural podría destruir un centro de datos, se podría obtener aplanado por un meteor, la línea de tronco en el centro de datos podrían eliminarse un productor burying una simple con una excavadora, etcetera. ¿Si la aplicación se hospeda en el centro de datos stricken qué se debe hacer? Es posible configurar la aplicación en Azure para ejecutar simultáneamente en varias regiones para que si se produce un desastre en uno, seguir ejecutándose en otra región. Estos errores son muy raros y no saltar la mayoría de las aplicaciones a través de los obstáculos necesarias para garantizar un servicio ininterrumpido a través de los errores de este tipo. Vea la sección de recursos al final del capítulo para obtener información sobre cómo mantener la aplicación esté disponible incluso a través de un error de región.

Es un objetivo de Azure administrar todos estos tipos de errores mucho más fáciles, y podrá ver algunos ejemplos de cómo lo estamos haciendo en los capítulos siguientes.

## <a name="slas"></a>SLA

A menudo, personas escuchan sobre contratos de nivel de servicio (SLA) en el entorno de nube. Básicamente, estas son promesas a las que las compañías tomar acerca del grado de confiabilidad de su servicio. Un 99,9% SLA significa que debe esperar a que el servicio funciona correctamente durante el 99,9% del tiempo. Que es un valor bastante típico para un SLA y eso suena como un número muy elevado, pero podrían no saber cuánto tiempo de inactividad. 1% realmente equivale a. Esta es una tabla que muestra cuánto tiempo de inactividad amount, que varios porcentajes de SLA a través de un año, mes y una semana.

![Tabla de SLA](design-to-survive-failures/_static/image2.png)

Por lo tanto, un 99,9% SLA significa que el servicio podría estar desconectado 8,76 horas 43,2 minutos al mes o un año. Que es más tiempo de inactividad de darse cuenta de la mayoría de los usuarios. Por lo tanto como un desarrollador desee tenga en cuenta que una determinada cantidad de tiempo de inactividad posible y controlarla de forma correcta. En algún momento alguien va a usar la aplicación, un servicio va a estar fuera de servicio y desea minimizar el impacto negativo de en el cliente.

¿Una cosa que debe conocer un SLA es qué período de tiempo hace referencia a: se restablezcan el reloj cada semana, cada mes o cada año? En Azure se restablece el reloj de cada mes, que es mejor para usted que un SLA anual, puesto que un SLA anual podría ocultar meses incorrectas desviando con una serie de meses buena.

Por supuesto siempre aspiramos a hacerlo mejor que el SLA; normalmente estará inactivo mucho menor. El compromiso es que si alguna vez estamos inactivo durante más tiempo que el máximo tiempo de inactividad, puede pedir back dinero. La cantidad de dinero que recibimos probablemente no le compensa plenamente la repercusión empresarial del exceso de tiempo de inactividad, pero ese aspecto del SLA actúa como una directiva de cumplimiento y le permite saber que nos tomamos muy en serio.

### <a name="composite-slas"></a>SLA compuestos

Una cuestión importante que pensar cuando está viendo los SLA es el impacto del uso de varios servicios en una aplicación, y cada servicio tiene un SLA independiente. Por ejemplo, la aplicación Fix It utiliza Azure App Service Web Apps, Azure Storage y SQL Database. Estos son los números de SLA a partir de la fecha de que este libro electrónico se está escribiendo en diciembre de 2013:

![Base de datos SQL de sitio Web, almacenamiento, de SLA](design-to-survive-failures/_static/image3.png)

¿Qué es el máximo tiempo improductivo que se esperaría para la aplicación basándose en estos SLA de servicio? Es posible que piense que el tiempo de inactividad sería igual que el porcentaje del SLA peor o 99,9% en este caso. Eso sería true si todos los servicios de tres errores siempre al mismo tiempo, pero no es necesariamente lo que sucede realmente. Cada servicio pueden producir un error por separado en momentos diferentes, por lo que debe calcular el SLA compuesto mediante la multiplicación de los números de SLA individuales.

![SLA compuesto](design-to-survive-failures/_static/image4.png)

Por lo que la aplicación podría estar inactiva no solo 43,2 minutos al mes pero 3 veces esa cantidad, 108 minutos al mes: y seguir estando dentro de los límites de SLA de Azure.

Este problema no es único en Azure. En realidad nos proporciona la mejor nube SLA de cualquier servicio de nube disponible, y tendrá problemas similares que tratar con si usa servicios de nube de cualquier proveedor. Lo que esto resalta es la importancia de pensar en cómo puede diseñar la aplicación para controlar los errores de servicio inevitable correctamente, porque es posible que ocurran con la frecuencia suficiente afectar a sus clientes o usuarios.

### <a name="cloud-slas-compared-to-enterprise-down-time-experience"></a>SLA de nube en comparación con la experiencia del tiempo de inactividad de enterprise

A veces, dicen, "En mi aplicación de empresa nunca tengo estos problemas." Si se pregunte ¿cuánto tiempo de inactividad mes que tienen en realidad, normalmente dicen, "Bueno, produce ocasionalmente." Y si se pregunte ¿con qué frecuencia, admitir que "En ocasiones se deben realizar copias de seguridad o instalar un nuevo software de servidor o actualización." Por supuesto, que cuenta como los tiempo de inactividad. La mayoría de las aplicaciones empresariales a menos que sean especialmente crítico para la empresa están realmente inactivos durante más de la cantidad de tiempo permitido por el SLA de nuestro servicio. Pero cuando es el servidor y la infraestructura y es responsabilidad suya para él y en el control de él, tiende a sentirse menos angustia acerca de tiempos de inactividad. En un entorno de nube está depende de otra persona y no sabe qué está ocurriendo, por lo que es posible que tienden a atraer más preocupado por él.

Cuando una empresa consigue un mayor porcentaje de tiempo que obtendrá de una nube SLA, lo hacen invirtiendo mucho más dinero en hardware. Un servicio en la nube podría hacer pero tendría que cobra mucho más para sus servicios. En su lugar, aprovechar las ventajas de un servicio rentable y diseñar software para que los errores inevitables provocar una interrupción mínima a sus clientes. Su trabajo como un diseñador de aplicaciones en la nube no es tanto para evitar errores que se evite una catástrofe y hacerlo centrándose en software, no en hardware. Mientras que las aplicaciones empresariales se esfuerzan por maximizar el tiempo medio entre errores, se esfuerzan por aplicaciones en la nube minimizar el tiempo medio para recuperación.

### <a name="not-all-cloud-services-have-slas"></a>No todos los servicios en la nube tienen SLA

Tenga en cuenta también que no todos los servicios en la nube incluso tienen un SLA. Si la aplicación depende de un servicio sin ninguna garantía de tiempo de actividad, podría ser inactivo durante mucho más tiempo que se podría imaginar. Por ejemplo, si habilita el inicio de sesión en su sitio mediante un proveedor social como Facebook o Twitter, consulte con el proveedor de servicios para averiguar si hay un SLA y es posible que no es uno. Pero si el servicio de autenticación deja de funcionar o no puede admitir el volumen de solicitudes que se le, los clientes quedan bloqueados fuera de la aplicación. Podría estar desconectado durante días o más. Los creadores de una nueva aplicación esperaba cientos de millones de descargas y tenía una dependencia en la autenticación de Facebook: pero no se comunican a Facebook antes de entrar en directo y detectadas demasiado tarde que se ha producido ningún SLA para ese servicio.

### <a name="not-all-downtime-counts-toward-slas"></a>No todos los tiempos de inactividad se descuenta SLA

Algunos servicios de nube deliberadamente pueden denegar el servicio si la aplicación los utiliza exceso. Esto se denomina *limitación*. Si un servicio tiene un SLA, debería indicar las condiciones en las que podría limitarse y diseño de su aplicación debe evitar esas condiciones y reaccionar de manera apropiada para la limitación si se produce. Por ejemplo, si las solicitudes a un servicio se inicia un error cuando se supera un cierto número por segundo, desea asegurarse de que los reintentos automáticos no ocurren tan rápido que hacen que la limitación continuar. Tendremos más que decir sobre la limitación en el [capítulo de control de errores transitorios](transient-fault-handling.md).

## <a name="summary"></a>Resumen

Este capítulo ha intentado le ayudan a comprender por qué una aplicación de nube del mundo real debe diseñarse para sobrevivir a fallos de forma correcta. A partir de la [siguiente capítulo](monitoring-and-telemetry.md), los demás patrones de esta serie incluyen más detalles sobre algunas estrategias que puede usar para hacer que:

- Tiene buen [supervisión y telemetría](monitoring-and-telemetry.md), de modo que descubra rápidamente los errores que requieren intervención y tiene suficiente información para resolverlos.
- [Controlar errores transitorios](transient-fault-handling.md) implementando lógica de reintento inteligentes, para que la aplicación recupera automáticamente cuando se puede y se vuelve a [disyuntor](transient-fault-handling.md#circuitbreakers) lógica cuando no es posible.
- Use [almacenamiento en caché distribuido](distributed-caching.md) con el fin de minimizar los problemas de rendimiento, latencia y la conexión con acceso de la base de datos.
- Implemente un acoplamiento a través de flexible el [patrón centrado en colas de trabajo](queue-centric-work-pattern.md), de modo que el front-end de aplicación puede seguir trabajando cuando el back-end está inactivo.

## <a name="resources"></a>Recursos

Para obtener más información, consulte los capítulos posteriores de este libro electrónico y los siguientes recursos.

Documentación:

- [Failsafe: Instrucciones para crear arquitecturas de nube resistentes](https://msdn.microsoft.com/library/windowsazure/jj853352.aspx). Notas del producto, Marc Mercuri, Ulrich Homann y Andrew Townhill. Versión de la serie de vídeos a prueba de errores de página Web.
- [Procedimientos recomendados para el diseño de servicios a gran escala en Azure Cloud Services](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). Notas del producto, Mark Simms y Michael Thomassy.
- [Guía técnica sobre la continuidad de negocio de Azure](https://msdn.microsoft.com/library/windowsazure/hh873027.aspx). Notas del producto, Patrick Wickline y Jason Roth.
- [Recuperación ante desastres y alta disponibilidad para las aplicaciones de Azure](https://msdn.microsoft.com/library/windowsazure/dn251004.aspx). Notas del producto por Michael McKeown, Hanu Kommalapati y Jason Roth.
- [Microsoft Patterns and Practices - Guía de Azure](https://msdn.microsoft.com/library/dn568099.aspx). Consulte las instrucciones de implementación de centro de datos múltiple, patrón de interruptor.
- [Soporte técnico de Azure - contratos de nivel de servicio](https://azure.microsoft.com/support/legal/sla/).
- [Continuidad del negocio en Azure SQL Database](https://msdn.microsoft.com/library/windowsazure/hh852669.aspx). Documentación sobre base de datos SQL alta ante desastres y disponibilidad de características de recuperación.
- [Alta disponibilidad y recuperación ante desastres para SQL Server en Azure Virtual Machines](https://msdn.microsoft.com/library/windowsazure/jj870962.aspx).

Vídeos:

- [FailSafe: Creación de servicios de nube escalables, resistentes](https://channel9.msdn.com/Series/FailSafe). Serie de nueve partes, Ulrich Homann, Marc Mercuri y Mark Simms. Presenta conceptos y principios de la arquitectura de una manera muy accesible e interesante, con casos procedentes de la experiencia del equipo de asesoramiento al cliente (CAT) de Microsoft con clientes reales. Episodios 1 y 8 profundizan en profundidad las razones para diseñar aplicaciones en la nube para sobrevivir a errores. Consulte también el análisis de seguimiento de la limitación en el episodio 2 empezando en la explicación de los disyuntores, episodio 3 empieza en 40:55, la discusión de puntos de error y modos de error en el episodio 2 comenzando en 56:05 y 49:57.
- [Creación Big: Lecciones aprendidas de los clientes de Azure: parte II](https://channel9.msdn.com/Events/Build/2012/3-030). Mark Simms habla sobre el diseño de error y la instrumentación de todo el contenido. Similar a la serie Failsafe pero entran en obtener más información sobre procedimientos.

> [!div class="step-by-step"]
> [Anterior](unstructured-blob-storage.md)
> [Siguiente](monitoring-and-telemetry.md)
