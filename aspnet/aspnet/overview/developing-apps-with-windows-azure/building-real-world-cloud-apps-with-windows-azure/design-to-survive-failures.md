---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/design-to-survive-failures
title: Diseño de supervivencia de errores (creación de aplicaciones de nube reales con Azure) | Documentos de Microsoft
author: MikeWasson
description: Las aplicaciones de nube de creación Real World con libros electrónicos Azure se basa en una presentación desarrollada por Scott Guthrie. Se explican 13 patrones y prácticas recomendadas que puede...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: 364ce84e-5af8-4e08-afc9-75a512b01f84
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/design-to-survive-failures
msc.type: authoredcontent
ms.openlocfilehash: 01883cb0be3e7c7b5dc8d32b784ccb3a28652f1e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30874114"
---
<a name="design-to-survive-failures-building-real-world-cloud-apps-with-azure"></a>Diseñar para sobreviven a errores (creación de aplicaciones de nube reales con Azure)
====================
por [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Descarga corregirlo proyecto](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) o [descargar libros electrónicos](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> El **compilar aplicaciones de nube de mundo Real con Azure** libros electrónicos se basa en una presentación desarrollada por Scott Guthrie. Se explican los 13 patrones y prácticas recomendadas que le permitirá tener éxito el desarrollo de aplicaciones web para la nube. Para obtener información sobre el libro electrónico, consulte [el primer capítulo](introduction.md).


Una de las acciones que tiene que preocuparse al compilar cualquier tipo de aplicación, pero sobre todo en uno que se ejecutará en la nube que muchas personas lo va a usar, se muestra cómo diseñar la aplicación para que pueda controlar los errores correctamente y sigan entregando valor tanto como es posible. Con el tiempo suficiente, lo va a algún problema en cualquier entorno o en cualquier sistema de software. Cómo la aplicación controla esas situaciones determina cómo abrumado obtendrá los clientes y cuánto tiempo tendrá que dedicar analizar y solucionar problemas.

## <a name="types-of-failures"></a>Tipos de errores

Existen dos categorías básicas de los errores que desea administrar de manera diferente:

- Transitorio, la recuperación automática de errores, como problemas de conectividad de red intermitentes.
- Permanentes con errores que requieren la intervención.

Para errores transitorios, puede implementar una directiva de reintentos para asegurarse de que la mayor parte de la hora de la aplicación recupera rápida y automática. Los clientes pueden observar ligeramente mayor tiempo de respuesta, pero en caso contrario no se verá afectadas. Le mostraremos algunas maneras de controlar estos errores en el [capítulo de administración de errores transitorios](transient-fault-handling.md).

Para permanentes con errores, puede implementar la supervisión y la funcionalidad que le notifica rápidamente cuando surjan problemas y que facilita el análisis de causa raíz del registro. Le mostraremos algunas maneras de ayudarle a mantenerse encima de estos tipos de errores en el [capítulo de supervisión y telemetría](monitoring-and-telemetry.md).

## <a name="failure-scope"></a>Ámbito de error

También tiene que pensar en el ámbito de error: si se ve afectado un único equipo, un servicio completo como base de datos SQL o almacenamiento o toda una región.

![Ámbito de error](design-to-survive-failures/_static/image1.png)

### <a name="machine-failures"></a>Errores de equipo

En Azure, un servidor que ha fallado se sustituye automáticamente por una nueva y, una aplicación de nube bien diseñada se recupera de este tipo de error de forma rápida y automática. Anteriormente se hincapié en las ventajas de escalabilidad de un nivel de web sin estado y la facilidad de recuperación desde un servidor que ha fallado es otra ventaja de no disponibilidad de Estados. Facilidad de recuperación también es una de las ventajas de las características de plataforma como servicio (PaaS) como base de datos SQL y aplicaciones de Web del servicio de aplicación de Azure. Errores de hardware son poco frecuentes, pero cuando se producen que estos servicios controlen automáticamente; incluso no tener que escribir código para controlar los errores de la máquina cuando se usa uno de estos servicios.

### <a name="service-failures"></a>Errores de servicio

Las aplicaciones de nube normalmente usan varios servicios. Por ejemplo, la aplicación repararlo utiliza el servicio de base de datos SQL, el servicio de almacenamiento y la aplicación web se implementa en el servicio de aplicaciones de Azure. ¿La aplicación lo que sucederá si se produce un error en uno de los servicios que depende de? Algunos errores de servicio que descriptivo "lo sentimos, inténtelo de nuevo más tarde" mensaje podría ser la mejor que puede hacer. Pero en muchos escenarios, puede hacer mejor. Por ejemplo, cuando el almacén de datos back-end está inactivo, puede aceptar la entrada de usuario, mostrar "se ha recibido la solicitud" y almacenar la entrada en algún lugar else temporalmente; después, cuando el servicio que necesite vuelve a estar operativo, puede recuperar la entrada y procesarlo.

El [centrada en la cola de trabajo de patrón](queue-centric-work-pattern.md) capítulo muestra una manera de controlar este escenario. La aplicación repararlo almacena las tareas en la base de datos SQL, pero no tiene que salir de funcionar cuando la base de datos SQL está inactivo. En este capítulo, veremos cómo almacenar proporcionados por el usuario para una tarea en una cola y usar un proceso de trabajo para leer la cola y actualizar la tarea. Si SQL está inactivo, la capacidad para crear tareas repararlo se ve afectada; el proceso de trabajo puede esperar y procesar las nuevas tareas cuando la base de datos SQL está disponible.

### <a name="region-failures"></a>Errores de región

Regiones enteras pueden producir errores. Un desastre natural podría destruir un centro de datos, se podría obtener aplanado por un meteor, la línea de tronco en el centro de datos se puede cortar un productor burying una vaca con backhoe, etcetera. Si la aplicación se hospeda en el centro de datos stricken ¿qué hace? Es posible configurar la aplicación en Azure para ejecutar simultáneamente en varias regiones de modo que si se produce un desastre en uno, continuar ejecutando en otra región. Estos errores son muy rara vez y mayoría de las aplicaciones no ir por el el aro necesario para garantizar un servicio ininterrumpido a través de los errores de este tipo. Vea la sección de recursos al final del capítulo para obtener información acerca de cómo mantener la aplicación disponible incluso a través de un error de región.

Es un objetivo de Azure administrar todos estos tipos de errores de mucho más sencillo y podrá ver algunos ejemplos de cómo estamos haciendo en los capítulos siguientes.

## <a name="slas"></a>SLA

Personas a menudo enterarse de contratos de nivel de servicio (SLA) en el entorno de nube. Básicamente, se trata de promesas que hacen que las empresas acerca del grado de confiabilidad de su servicio. Un 99,9% SLA significa que debe esperar a que el servicio funciona correctamente un 99,9% del tiempo. Que es un valor bastante típico para un SLA y puede que parezca un número muy elevado, pero podrían no saber cuánto tiempo de inactividad. 1% realmente equivale a. Aquí es una tabla que muestra cuánto tiempo de inactividad varios porcentajes de SLA equivalen a través de un año, mes y a la semana.

![Tabla de SLA](design-to-survive-failures/_static/image2.png)

Por lo tanto un 99,9% SLA significa que el servicio podría estar desconectado 8,76 horas 43,2 minutos al mes o un año. Que es más tiempo de inactividad que muchas personas les creían asignados. Por lo tanto como un programador desea tener en cuenta que es posible una cierta cantidad de tiempo de inactividad y controlar de forma correcta. En algún momento alguien se va a usar la aplicación y un servicio se va a estar inactiva y desea minimizar el impacto negativo de en el cliente.

¿Único lo que debe conocer un SLA es qué período de tiempo que hace referencia: se restablezca el reloj cada semana, cada mes o cada año? En Azure se restablece el reloj de cada mes, lo que es mejor para usted que un SLA anual, puesto que un SLA anual habiendo meses incorrectas por compensación con una serie de meses buena.

Por supuesto se siempre aspirar a hacer un mejor rendimiento que el SLA; normalmente estará presionada mucho menor. El compromiso es que si alguna vez estamos inactivo durante más tiempo que el máximo tiempo de inactividad puede pedir back dinero. La cantidad de dinero que regresar probablemente no compensa completamente el impacto de negocio del exceso de tiempo de inactividad, pero ese aspecto del SLA actúa como una directiva de cumplimiento y le permite saber que se toma muy en serio.

### <a name="composite-slas"></a>SLA compuestos

Una cuestión importante que pensar cuando encuentre SLA es el impacto del uso de varios servicios en una aplicación, con cada servicio tiene un SLA independiente. Por ejemplo, la aplicación repararlo utiliza aplicaciones de Web del servicio de aplicación de Azure, almacenamiento de Azure y base de datos SQL. Estas son sus números de SLA hasta la fecha en que se está escribiendo este libro electrónico en diciembre de 2013:

![Base de datos SQL de sitio Web, almacenamiento, de SLA](design-to-survive-failures/_static/image3.png)

¿Cuál es el máximo tiempo improductivo que se esperaría para la aplicación en función de estos SLA de servicio? Puede que piense que su tiempo de inactividad sea igual que el porcentaje del SLA peor o 99,9% en este caso. Que sería true si los tres servicios no pudieron siempre al mismo tiempo, pero no es necesariamente lo que sucede realmente. Cada servicio puede producir un error por separado en momentos diferentes, por lo que hay que calcular el SLA compuesto mediante la multiplicación de los números de SLA individuales.

![SLA compuesto](design-to-survive-failures/_static/image4.png)

Por lo que la aplicación podría estar inactiva no solo 43,2 minutos un mes pero 3 veces esa cantidad, 108 minutos al mes: y seguir siendo dentro de los límites de SLA de Azure.

Este problema no es exclusivo de Azure. Proporcionamos realmente la mejor nube SLA de cualquier servicio de nube disponible, y tendrá problemas similares para tratar con si utiliza los servicios de nube de cualquier proveedor. ¿Qué es muy es la importancia de pensar en cómo se puede diseñar la aplicación para tratar los errores de servicio inevitable correctamente, ya que pueden ocurrir con la frecuencia suficiente afectar a sus clientes o usuarios.

### <a name="cloud-slas-compared-to-enterprise-down-time-experience"></a>SLA de nube en comparación con la experiencia de tiempo de inactividad de enterprise

A veces, dicen, "En mi aplicación de empresa nunca tengo estos problemas." Si solicita un mes cuánto tiempo de inactividad que tienen en realidad, dicen normalmente, ", se produce de vez en cuando." Y si solicita la frecuencia, admitir que "En ocasiones es necesario hacer una copia o instalar un nuevo software de servidor o update." Por supuesto, que cuenta como los tiempos de inactividad. Mayoría de las aplicaciones empresariales a menos que sean especialmente críticas está realmente inactivos durante más de la cantidad de tiempo permitido por los SLA de servicio. Pero cuando es el servidor y la infraestructura y es responsable de él y en el control de él, tiende a sentirse menos angustia acerca de tiempo de inactividad. En un entorno de nube es dependiente de otra persona y no sabe que está sucediendo, por lo que podría tienden a obtener más preocupada sobre él.

Cuando una empresa consigue un mayor porcentaje de tiempo de actividad que obtendrá de una SLA de nube, lo hacen por dinero mucho más los gastos en hardware. Un servicio de nube podría hacer pero tendría que cobran mucho más para sus servicios. En su lugar, aprovechar las ventajas de un servicio rentable y diseñar el software para que los errores inevitables provocar interrupciones mínimo a sus clientes. Su trabajo como un diseñador de aplicaciones de nube no es tanto para evitar errores que se eviten catástrofe y hacerlo centrándose en software, no en hardware. Mientras que se esfuerzan por aplicaciones empresariales maximizar el tiempo medio entre errores, las aplicaciones de nube lo posible para minimizar el tiempo promedio para recuperar.

### <a name="not-all-cloud-services-have-slas"></a>No todos los servicios de nube tienen SLA

Tenga en cuenta también que no todos los servicios en la nube incluso tiene un SLA. Si la aplicación depende de un servicio con ninguna garantía de tiempo de actividad, podría estar desconectado mucho más tiempo del que se pueda imaginar. Por ejemplo, si habilita el inicio de sesión en su sitio mediante un proveedor de redes sociales como Facebook o Twitter, compruebe con el proveedor de servicios para averiguar si hay un SLA, y puede ser disponible no es uno. Pero si el servicio de autenticación deja de funcionar o no puede admitir el volumen de solicitudes que producirá en él, los clientes quedan bloqueados fuera de la aplicación. Podría estar desconectado durante días o más. Los creadores de una nueva aplicación esperaba centenares de millones de descargas y adoptó una dependencia en la autenticación de Facebook: pero no se comunican a Facebook antes de pasar demasiado tarde en vivo y detectados que no hubo ningún SLA para ese servicio.

### <a name="not-all-downtime-counts-toward-slas"></a>No todos los tiempos de inactividad se descuenta SLA

Algunos servicios de nube deliberadamente pueden denegar el servicio si usa exceso con su aplicación. Esto se denomina *limitación*. Si un servicio tiene un SLA, deberían indicar las condiciones en las que podría ser limitada y el diseño de la aplicación debe evitar esas condiciones y reaccionar correctamente a la limitación de peticiones si se produce. Por ejemplo, si las solicitudes a un servicio se inicia un error cuando se supera un cierto número por segundo, desea asegurarse de que los reintentos automáticos no ocurren rápido que hacen que la limitación de peticiones continuar. Tendremos más que decir sobre limitación en el [capítulo de administración de errores transitorios](transient-fault-handling.md).

## <a name="summary"></a>Resumen

En este capítulo probada para ayudarle a obtener por qué una aplicación de nube del mundo real debe diseñarse para que sobreviven a errores correctamente. A partir de la [siguiente capítulo](monitoring-and-telemetry.md), los patrones restantes de esta serie incluyen más detalles sobre algunas estrategias que puede usar para realizar dicha acción:

- Tiene buen [supervisión y telemetría](monitoring-and-telemetry.md), de modo que descubra rápidamente acerca de los errores que requieran la intervención, y tiene suficiente información para solucionarlos problemas asociados.
- [Controlar errores transitorios](transient-fault-handling.md) implementando lógica de reintento inteligentes, para que la aplicación recupera automáticamente cuando pueda y vuelve a [disyuntor](transient-fault-handling.md#circuitbreakers) lógica cuando no es posible.
- Use [caching distribuido](distributed-caching.md) con el fin de minimizar los problemas de rendimiento, latencia y la conexión con acceso de base de datos.
- Implementar a través de acoplamiento flexible la [patrón centrada en la cola de trabajo](queue-centric-work-pattern.md), de modo que el front-end de aplicación puede seguir trabajando cuando el back-end no está operativo.

## <a name="resources"></a>Recursos

Para obtener más información, consulte los capítulos posteriores de este libro electrónico y los siguientes recursos.

Documentación:

- [Failsafe: Instrucciones para crear arquitecturas de nube resistentes](https://msdn.microsoft.com/library/windowsazure/jj853352.aspx). Notas del producto Marc Mercuri, Ulrich Homann y Andrew Townhill. Versión de la página Web de la serie de vídeos de FailSafe.
- [Procedimientos recomendados para el diseño de servicios a gran escala en los servicios de nube de Azure](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). Notas del producto, Mark Simms y Michael Thomassy.
- [Orientación técnica de la continuidad de negocio de Azure](https://msdn.microsoft.com/library/windowsazure/hh873027.aspx). Notas del producto, Patrick Wickline y Jason Roth.
- [Recuperación ante desastres y alta disponibilidad para las aplicaciones de Azure](https://msdn.microsoft.com/library/windowsazure/dn251004.aspx). Notas del producto por Michael McKeown y Hanu Kommalapati, Jason Roth.
- [Microsoft patrones y prácticas - Guía de Azure](https://msdn.microsoft.com/library/dn568099.aspx). Consulte las instrucciones de implementación de centro de datos de varias, el patrón de disyuntor.
- [Soporte técnico de Azure - contratos de nivel de servicio](https://azure.microsoft.com/support/legal/sla/).
- [Continuidad del negocio en base de datos SQL Azure](https://msdn.microsoft.com/library/windowsazure/hh852669.aspx). Documentación acerca de la base de datos SQL alta disponibilidad y ante desastres características de recuperación.
- [Alta disponibilidad y recuperación ante desastres para SQL Server en máquinas virtuales de Azure](https://msdn.microsoft.com/library/windowsazure/jj870962.aspx).

Vídeos:

- [FailSafe: Creación de servicios en la nube escalables y resistentes](https://channel9.msdn.com/Series/FailSafe). Serie de nueve partes por Ulrich Homann y Marc Mercuri, Mark Simms. Presenta los conceptos y principios de la arquitectura de una manera muy interesante y accesible, con casos extraídos de la experiencia del equipo de asesoramiento al cliente (CAT) de Microsoft con clientes reales. Episodios 1 y 8 profundizan en los motivos para diseñar aplicaciones de nube que sobreviven a errores. Vea también el análisis de seguimiento de la limitación en episodio 2 a partir de 49:57, la explicación de los puntos de error y modos de error episodio 2 empieza en 56:05 y la explicación de los disyuntores episodio 3 empieza en 40:55.
- [Creación Big: Lecciones aprendidas de clientes de Azure, parte II](https://channel9.msdn.com/Events/Build/2012/3-030). Mark Simms habla sobre el diseño de error y la instrumentación de todo el contenido. Similar a la serie Failsafe pero queda procedimientos con mayor detalle.

> [!div class="step-by-step"]
> [Anterior](unstructured-blob-storage.md)
> [Siguiente](monitoring-and-telemetry.md)
