---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/distributed-caching
title: Almacenamiento en caché (crear aplicaciones de nube reales con Azure) distribuido | Microsoft Docs
author: MikeWasson
description: Building Real World Cloud Apps with e-book de Azure se basa en una presentación desarrollada por Scott Guthrie. Explican el 13 de patrones y prácticas que puede...
ms.author: riande
ms.date: 07/20/2015
ms.assetid: 406518e9-3817-49ce-8b90-e82bc461e2c0
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/distributed-caching
msc.type: authoredcontent
ms.openlocfilehash: 1f67c754fcc03462c25983a8397a1093997b1213
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838288"
---
<a name="distributed-caching-building-real-world-cloud-apps-with-azure"></a>Almacenamiento en caché (compilación reales en la nube aplicaciones distribuidas con Azure)
====================
por [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Descargar proyecto corregirlo](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) o [descargar libro electrónico](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> El **Building Real World Cloud Apps with Azure** eBook se basa en una presentación desarrollada por Scott Guthrie. Se explican el 13 patrones y prácticas que pueden ayudarle a tener éxito el desarrollo de aplicaciones web para la nube. Para obtener información sobre el libro electrónico, consulte [el primer capítulo](introduction.md).


El capítulo anterior había examinado el control de errores transitorios y mencionó el almacenamiento en caché como una estrategia de interruptor. Este capítulo ofrece más información sobre el almacenamiento en caché, incluido cuándo usarlo, patrones comunes para usar y cómo implementarla en Azure.

## <a name="what-is-distributed-caching"></a>¿Qué se distribuye el almacenamiento en caché

Una memoria caché proporciona alto rendimiento, baja latencia de acceso a datos de la aplicación a los que accede con frecuencia, almacenando los datos en memoria. Para una aplicación de nube el tipo de caché más útil es una caché distribuida, lo que significa que los datos no se almacenan en memoria del servidor web individuales, pero en otros recursos en la nube y los datos en caché está disponible para todos los servidores de la aplicación web (u otras máquinas virtuales de nube que ar e utilizado por la aplicación).

![diagrama que muestra varios servidores web acceso a los mismos servidores de caché](distributed-caching/_static/image1.png)

Cuando la aplicación se escala mediante la adición o eliminación de servidores, o cuando se reemplazan los servidores debido a las actualizaciones o errores, los datos en caché sigue siendo accesibles para todos los servidores que se ejecuta la aplicación.

Al evitar el acceso a datos de latencia alta de un almacén de datos persistente, almacenamiento en caché puede mejorar considerablemente la capacidad de respuesta de la aplicación. Por ejemplo, la recuperación de datos de la memoria caché es mucho más rápido que recuperarlos desde una base de datos relacional.

Un beneficio colateral del almacenamiento en caché se reduce el tráfico al almacén de datos persistente, lo que puede provocar disminuye los costos cuando hay salida de los datos se cobra por el almacén de datos persistente.

## <a name="when-to-use-distributed-caching"></a>Cuándo se debe usar el almacenamiento en caché distribuido

Almacenamiento en caché funciona mejor para las cargas de trabajo de aplicación que no leer más que la escritura de datos, y cuando el modelo de datos es compatible con la organización de pares clave-valor que usa para almacenar y recuperar datos en memoria caché. También es más útil cuando los usuarios de aplicaciones comparten una gran cantidad de datos comunes; Por ejemplo, memoria caché no proporcionaría tantas ventajas si cada usuario normalmente recupera datos únicos para ese usuario. Un ejemplo donde almacenamiento en caché puede ser muy beneficioso es un catálogo de productos, dado que los datos no cambian con frecuencia, y todos los clientes buscan en los mismos datos.

La ventaja de almacenamiento en caché se convierte en medible cada vez más se escala una aplicación, los límites de rendimiento y los retrasos de latencia de almacén de datos persistente, se convierten en más de un límite en el rendimiento general de la aplicación. Sin embargo, podría implementar por otros motivos que así el rendimiento de almacenamiento en caché. Para los datos que no tienen que ser totalmente actualizado cuando se muestra al usuario, el acceso a la caché puede actuar como un disyuntor para cuando el almacén de datos persistente no responde o no está disponible.

## <a name="popular-cache-population-strategies"></a>Estrategias de población de caché populares

Para poder recuperar datos de caché, tendrá que almacenar allí en primer lugar. Existen varias estrategias para introducir los datos que necesita en una memoria caché:

- Petición / caché Aside

    La aplicación intenta recuperar datos de caché y cuando la memoria caché no tiene los datos (un "miss"), la aplicación almacena los datos en la memoria caché, por lo que estará disponible la próxima vez. La próxima vez que la aplicación intenta obtener los mismos datos, encuentra lo que está buscando en la memoria caché (un "acierto"). Para evitar la obtención de datos en caché que han cambiado en la base de datos, invalidar la memoria caché al realizar cambios en el almacén de datos.
- Inserción de datos en segundo plano

    Servicios en segundo plano inserción datos en la memoria caché en una programación regular y la aplicación siempre extrae de la memoria caché. Este enfoque funciona bien con orígenes de datos de alta latencia que no requieren siempre devuelve los datos más recientes.
- Interruptor de circuito

    Normalmente, la aplicación se comunica directamente con el almacén de datos persistentes, pero cuando el almacén de datos persistente tiene problemas de disponibilidad, la aplicación recupera datos de la memoria caché. Es posible que se han colocado datos en memoria caché mediante la reserva de caché o estrategia de inserción de datos en segundo plano. Se trata de un control de estrategia en lugar de una estrategia de mejora del rendimiento de errores.

Para mantener los datos en la memoria caché actual, puede eliminar las entradas de caché relacionada cuando la aplicación crea, actualiza, o elimina datos. Si bien es para que la aplicación para obtener a veces los datos que es un poco obsoletos, puede confiar en un tiempo de expiración configurables para establecer un límite de antigüedad caché pueden ser datos.

Puede configurar una expiración absoluta (cantidad de tiempo transcurrido desde que se creó el elemento de caché) o el plazo de expiración (cantidad de tiempo desde la última vez que se ha accedido a un elemento de caché). Cuando se según el mecanismo de expiración de caché para impedir que los datos se vuelva demasiado obsoleta, se utiliza una expiración absoluta. En la aplicación Fix It, se deberá expulsar manualmente elementos de la caché obsoletos y utilizaremos una expiración variable para mantener los datos más actuales en memoria caché. Independientemente de la directiva de expiración que elija, la memoria caché automáticamente expulsar los elementos más antiguos (menos usado recientemente o LRU) cuando se alcanza el límite de memoria de la memoria caché.

## <a name="sample-cache-aside-code-for-fix-it-app"></a>Ejemplo de código de cache-aside para aplicación Fix It

En el siguiente ejemplo de código, comprobamos la memoria caché en primer lugar al recuperar una tarea Fix It. Si la tarea se encuentra en la memoria caché, se devolverá; Si no lo encuentra, se obtenerlo de la base de datos y almacenarla en la memoria caché. Los cambios que realizaría para agregar almacenamiento en caché para el `FindTaskByIdAsync` método aparecen resaltados.

[!code-csharp[Main](distributed-caching/samples/sample1.cs?highlight=5,9-11,13-15,19)]

Al actualizar o eliminar una tarea Fix It, tendrá que invalidar tarea (quitar) almacenado en caché. En caso contrario, futuro intenta leer que esa tarea seguirá pudiendo obtener los datos antiguos de la memoria caché.

[!code-csharp[Main](distributed-caching/samples/sample2.cs?highlight=7)]

Estos son ejemplos que muestran el código simple de almacenamiento en caché; almacenamiento en caché no se ha implementado en el que se pueden descargar Fix It proyecto.

## <a name="azure-caching-services"></a>Servicios de almacenamiento en caché de Azure

Azure ofrece los siguientes servicios de almacenamiento en caché: [Azure Redis Cache](https://msdn.microsoft.com/library/dn690523.aspx) y [Azure Managed Cache](https://msdn.microsoft.com/library/dn386094.aspx). Azure Redis cache se basa en la conocida [caché Redis de código abierto](http://redis.io/) y es la primera opción para la mayoría de almacenamiento en caché los escenarios.

<a id="sessionstate"></a>
## <a name="aspnet-session-state-using-a-cache-provider"></a>Estado de sesión ASP.NET mediante un proveedor de caché

Como se mencionó en el [capítulo web desarrollo de mejores prácticas](web-development-best-practices.md), una práctica recomendada consiste en evitar utilizar el estado de sesión. Si la aplicación requiere que el estado de sesión, el siguiente procedimiento recomendado es evitar el proveedor en memoria predeterminada ya que no permiten el escalado horizontal (varias instancias del servidor web). El proveedor de estado de sesión de SQL Server de ASP.NET permite a un sitio que se ejecuta en varios servidores web para utilizar el estado de sesión, pero implica un costo de latencia alta en comparación con un proveedor en memoria. La mejor solución si tiene que utilizar el estado de sesión es utilizar un proveedor de caché, como el [proveedor de estado de sesión para Azure Cache](https://msdn.microsoft.com/library/windowsazure/gg185668.aspx).

## <a name="summary"></a>Resumen

Ha visto cómo la aplicación Fix It puede implementar con el fin de mejorar la escalabilidad y el tiempo de respuesta y para habilitar la aplicación seguir estando con capacidad de respuesta para las operaciones de lectura cuando no está disponible la base de datos de almacenamiento en caché. En el [siguiente capítulo](queue-centric-work-pattern.md) le mostraremos cómo mejorar la escalabilidad y hacer que la aplicación continúe tienen capacidad de respuesta para las operaciones de escritura.

## <a name="resources"></a>Recursos

Para obtener más información sobre almacenamiento en caché, consulte los siguientes recursos.

Documentación

- [Caché de Azure](https://msdn.microsoft.com/library/gg278356.aspx). Documentación oficial de MSDN sobre caching de Azure.
- [Microsoft Patterns and Practices - Guía de Azure](https://msdn.microsoft.com/library/dn568099.aspx). Consulte las instrucciones de almacenamiento en caché y el patrón Cache-Aside.
- [Failsafe: Instrucciones para crear arquitecturas de nube resistentes](https://msdn.microsoft.com/library/windowsazure/jj853352.aspx). Notas del producto, Marc Mercuri, Ulrich Homann y Andrew Townhill. Consulte la sección sobre el almacenamiento en caché.
- [Procedimientos recomendados para el diseño de servicios a gran escala en Azure Cloud Services](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). W. Notas del producto, Mark Simms y Michael Thomassy. Consulte la sección sobre almacenamiento en caché distribuido.
- [Almacenamiento en caché en la ruta de acceso para la escalabilidad distribuido](https://msdn.microsoft.com/magazine/dd942840.aspx). Un artículo de MSDN Magazine (2009) anterior, pero una introducción al almacenamiento en caché distribuido en general; claramente escrita entra en mayor profundidad que el almacenamiento en caché secciones de las notas del producto a prueba de errores y los procedimientos recomendados.

Vídeos

- [FailSafe: Creación de servicios de nube escalables, resistentes](https://channel9.msdn.com/Series/FailSafe). Serie de nueve partes, Ulrich Homann, Marc Mercuri y Mark Simms. Presenta una vista de nivel 400 de cómo diseñar aplicaciones en la nube. Esta serie se centra en la teoría y motivos de por qué; Para obtener más información sobre procedimientos, consulte la serie Building grande por Mark Simms. Vea la explicación de almacenamiento en caché episodio 3 empieza en 1:24:14.
- [Creación Big: Lecciones aprendidas de los clientes de Azure: parte I](https://channel9.msdn.com/Events/Build/2012/3-029). Simon Davies analiza iniciando almacenamiento en caché distribuido en 46:00. Similar a la serie Failsafe pero entran en obtener más información sobre procedimientos. La presentación se asignó el 31 de octubre de 2012, por lo que no cubre el servicio de almacenamiento en caché de Web Apps en Azure App Service que se introdujo en 2013.

Ejemplo de código

- [En la nube Fundamentos de servicio en Azure](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649). Aplicación de ejemplo que implementa el almacenamiento en caché distribuido. Consulte el blog que lo acompaña [Fundamentos de servicio en la nube: conceptos básicos de almacenamiento en caché](https://blogs.msdn.com/b/windowsazure/archive/2013/10/03/cloud-service-fundamentals-caching-basics.aspx).

> [!div class="step-by-step"]
> [Anterior](transient-fault-handling.md)
> [Siguiente](queue-centric-work-pattern.md)
