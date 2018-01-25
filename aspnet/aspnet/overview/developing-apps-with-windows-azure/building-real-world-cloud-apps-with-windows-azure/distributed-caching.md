---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/distributed-caching
title: "Distribuido en caché (creación de aplicaciones de nube reales con Azure) | Documentos de Microsoft"
author: MikeWasson
description: "Las aplicaciones de nube de creación Real World con libros electrónicos Azure se basa en una presentación desarrollada por Scott Guthrie. Se explican 13 patrones y prácticas recomendadas que puede..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/20/2015
ms.topic: article
ms.assetid: 406518e9-3817-49ce-8b90-e82bc461e2c0
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/distributed-caching
msc.type: authoredcontent
ms.openlocfilehash: 24ede9cb9289c84140f6e2573f9d526f19cac64b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="distributed-caching-building-real-world-cloud-apps-with-azure"></a>Almacenamiento en caché (compilación reales en la nube aplicaciones distribuidas con Azure)
====================
por [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Descarga corregirlo proyecto](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) o [descargar libros electrónicos](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> El **compilar aplicaciones de nube de mundo Real con Azure** libros electrónicos se basa en una presentación desarrollada por Scott Guthrie. Se explican los 13 patrones y prácticas recomendadas que le permitirá tener éxito el desarrollo de aplicaciones web para la nube. Para obtener información sobre el libro electrónico, consulte [el primer capítulo](introduction.md).


El capítulo anterior examinando el control de errores transitorios y se ha mencionado el almacenamiento en caché como una estrategia de disyuntor. En este capítulo proporciona más información sobre el almacenamiento en caché, incluyendo cuándo usarlo, patrones comunes para su uso y cómo implementarla en Azure.

## <a name="what-is-distributed-caching"></a>¿Qué se distribuye el almacenamiento en caché

Una memoria caché proporciona un rendimiento alto, baja latencia de acceso a los datos de aplicación que habitualmente, almacenando los datos en memoria. Para una aplicación de nube el tipo más útil de caché es caché distribuida, lo que significa que los datos no se almacenan en memoria del servidor web individuales, pero en otros recursos de nube y los datos almacenados en caché están disponibles para todos los servidores web de la aplicación (o Sí en la nube máquinas virtuales que ar e utilizado por la aplicación).

![diagrama que muestra varios servidores web obtiene acceso a los mismos servidores de caché](distributed-caching/_static/image1.png)

Cuando la aplicación se adapta al agregar o quitar servidores, o cuando se reemplazan los servidores debido a las actualizaciones o errores, los datos en caché siguen siendo accesibles a todos los servidores que se ejecuta la aplicación.

Al evitar el acceso a datos de latencia alta de un almacén de datos persistente, almacenamiento en caché puede mejorar considerablemente la capacidad de respuesta de la aplicación. Por ejemplo, la recuperación de datos desde la memoria caché es mucho más rápida que recuperar de una base de datos relacional.

Una ventaja colateral del almacenamiento en caché se reduce el tráfico en el almacén de datos persistentes, lo que puede conllevar menores costos cuando hay salida de datos se aplicarán cargos para el almacén de datos persistente.

## <a name="when-to-use-distributed-caching"></a>Cuándo se debe utilizar el almacenamiento en caché distribuido

Almacenamiento en caché funciona mejor para las cargas de trabajo de aplicación que no más de lectura a escritura de los datos, y cuando el modelo de datos sea compatible con la organización de clave/valor que usa para almacenar y recuperar datos en memoria caché. También es más útil cuando los usuarios de la aplicación comparten una gran cantidad de datos comunes; Por ejemplo, memoria caché no proporcionaría tantos beneficios si cada usuario normalmente recupera datos únicos para ese usuario. Un ejemplo donde el almacenamiento en caché puede ser muy beneficioso es un catálogo de productos, porque los datos no cambian con frecuencia, y todos los clientes se trata de los mismos datos.

La ventaja de almacenamiento en caché se convierte en cada vez más apreciable más se escala una aplicación, como los límites de rendimiento y los retrasos en la latencia de almacén de datos persistente, se convierten en más de un límite en el rendimiento general de la aplicación. Sin embargo, puede implementar el almacenamiento en caché por otras razones de rendimiento también. Para los datos que no tienen que ser perfectamente actualizada cuando se muestra al usuario, el acceso a la caché puede actuar como un disyuntor para cuando el almacén de datos persistentes es que no responde o no está disponible.

## <a name="popular-cache-population-strategies"></a>Estrategias de llenado de caché populares

Para poder recuperar datos de memoria caché, tiene que almacenar existe en primer lugar. Hay varias estrategias para obtener los datos que necesita en una memoria caché:

- A petición / caché Aside

    La aplicación intenta recuperar datos de memoria caché y cuando la memoria caché no tiene los datos (un "error"), la aplicación almacena los datos en la memoria caché, por lo que estará disponible la próxima vez. La próxima vez que la aplicación intenta obtener los mismos datos, encuentra lo que está buscando en la memoria caché (un "acierto"). Para evitar la obtención de datos almacenados en caché que han cambiado en la base de datos, se invalidan la caché al realizar cambios en el almacén de datos.
- Inserción de datos de fondo

    Servicios en segundo plano inserción datos en la memoria caché en una programación regular y la aplicación siempre extrae de la memoria caché. Este enfoque funciona bien con orígenes de datos de latencia alta que no requieren siempre devuelve los datos más recientes.
- Disyuntor

    La aplicación normalmente se comunica directamente con el almacén de datos persistentes, pero cuando el almacén de datos persistente tiene problemas de disponibilidad, la aplicación recupera datos de caché. Datos puedan haberse colocados en memoria caché con la reserva de caché o la estrategia de inserción de datos de fondo. Se trata de un error de control de estrategia en lugar de una estrategia de mejora del rendimiento.

Para conservar los datos en la memoria caché actual, puede eliminar las entradas de caché relacionados cuando la aplicación cree, las actualizaciones, o elimine datos. Si es correcta la aplicación puede recibir a veces los datos que es un poco obsoletos, puede basarse en una hora de caducidad se puede configurar para establecer un límite en la memoria caché la antigüedad de los datos pueden ser.

Puede configurar la expiración absoluta (período de tiempo desde que se creó el elemento en caché) o el plazo de expiración (período de tiempo desde la última vez que se tiene acceso a un elemento en caché). Expiración absoluta se usa cuando se está dependiendo de que el mecanismo de expiración de caché para evitar que los datos vuelvan obsoletos. En la aplicación repararlo, se dejarán expulsar manualmente elementos de la caché obsoletos y usaremos la expiración variable para mantener los datos más actuales en caché. Independientemente de la directiva de expiración que elija, la memoria caché expulsará automáticamente los elementos más antiguos (menos usado recientemente o LRU) cuando se alcanza el límite de memoria de la memoria caché.

## <a name="sample-cache-aside-code-for-fix-it-app"></a>Código de ejemplo caché-aside para repararlo aplicación

En el siguiente ejemplo de código, comprobamos la memoria caché en primer lugar al recuperar una tarea repararlo. Si la tarea se encuentra en la memoria caché, devolvemos Si no se encuentra, se obtener de la base de datos y almacenarla en la memoria caché. Los cambios que se realice agregar almacenamiento en caché a la `FindTaskByIdAsync` método aparecen resaltados.

[!code-csharp[Main](distributed-caching/samples/sample1.cs?highlight=5,9-11,13-15,19)]

Al actualizar o eliminar una tarea repararlo, tendrá que invalidar tarea (quitar) las almacenadas en caché. En caso contrario, futuro intenta leer que esa tarea seguirá pudiendo obtener los datos antiguos de la memoria caché.

[!code-csharp[Main](distributed-caching/samples/sample2.cs?highlight=7)]

Se trata de ejemplos que muestran el código de almacenamiento en caché simple; almacenamiento en caché no se ha implementado en el que se puede descargar repararlo proyecto.

## <a name="azure-caching-services"></a>Almacenamiento en caché de servicios de Azure

Azure ofrece los siguientes servicios de almacenamiento en caché: [Azure Redis Cache](https://msdn.microsoft.com/library/dn690523.aspx) y [caché administrada de Azure](https://msdn.microsoft.com/library/dn386094.aspx). Caché en Redis de Azure se basa en la conocida [Abrir origen de caché en Redis](http://redis.io/) y es la primera opción para la mayoría, almacenamiento en caché escenarios.

<a id="sessionstate"></a>
## <a name="aspnet-session-state-using-a-cache-provider"></a>Estado de sesión ASP.NET usando un proveedor de caché

Como se mencionó en la [capítulo de prácticas recomendada de web development](web-development-best-practices.md), una práctica recomendada consiste en evitar utilizar el estado de sesión. Si la aplicación requiere que el estado de sesión, el siguiente procedimiento recomendado es evitar el proveedor en memoria de forma predeterminada, ya que no permiten escalar horizontalmente (varias instancias del servidor web). El proveedor de estado de sesión de SQL Server de ASP.NET permite a un sitio que se ejecuta en varios servidores web para utilizar el estado de sesión, pero implica un costo de latencia alta en comparación con un proveedor en memoria. La mejor solución si tiene que usar el estado de sesión es usar un proveedor de caché, como el [proveedor de estado de sesión para caché de Azure](https://msdn.microsoft.com/library/windowsazure/gg185668.aspx).

## <a name="summary"></a>Resumen

Ha visto cómo podría implementar la aplicación repararlo almacenamiento en caché con el fin de mejorar la escalabilidad y el tiempo de respuesta y para permitir que la aplicación siga siendo con capacidad de respuesta para las operaciones de lectura cuando la base de datos no está disponible. En el [siguiente capítulo](queue-centric-work-pattern.md) le mostraremos cómo mejorar la escalabilidad y hacer que la aplicación continúe capacidad de respuesta para las operaciones de escritura.

## <a name="resources"></a>Recursos

Para obtener más información sobre el almacenamiento en caché, vea los siguientes recursos.

Documentación

- [Caché de Azure](https://msdn.microsoft.com/library/gg278356.aspx). Documentación de MSDN oficial en almacenamiento en caché de Azure.
- [Microsoft patrones y prácticas - Guía de Azure](https://msdn.microsoft.com/library/dn568099.aspx). Consulte las instrucciones de almacenamiento en caché y modelo Cache-Aside.
- [Failsafe: Instrucciones para crear arquitecturas de nube resistentes](https://msdn.microsoft.com/library/windowsazure/jj853352.aspx). Notas del producto Marc Mercuri, Ulrich Homann y Andrew Townhill. Vea la sección sobre el almacenamiento en caché.
- [Procedimientos recomendados para el diseño de servicios a gran escala en los servicios de nube de Azure](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). W. Notas del producto, Mark Simms y Michael Thomassy. Vea la sección sobre el almacenamiento en caché distribuido.
- [Distribuido en caché en la ruta de acceso de escalabilidad](https://msdn.microsoft.com/magazine/dd942840.aspx). Un artículo de MSDN Magazine (2009) anterior, pero una escritas con claridad Introducción al almacenamiento en caché distribuido en general; entra en más detalle en las secciones de almacenamiento en caché de las notas del producto FailSafe y procedimientos recomendados.

Vídeos

- [FailSafe: Creación de servicios en la nube escalables y resistentes](https://channel9.msdn.com/Series/FailSafe). Serie de nueve partes por Ulrich Homann y Marc Mercuri, Mark Simms. Presenta una vista de nivel 400 sobre cómo diseñar aplicaciones de nube. Esta serie se centra en la teoría y razones por; Para obtener más información sobre procedimientos, vea la serie edificio grande por Mark Simms. Vea la explicación de almacenamiento en caché en episodio 3 empezando por 1:24:14.
- [Creación Big: Lecciones aprendidas de clientes de Azure, parte I](https://channel9.msdn.com/Events/Build/2012/3-029). Simon Davies describe distribuida almacenamiento en caché a partir de 46:00. Similar a la serie Failsafe pero queda procedimientos con mayor detalle. La presentación se asignó el 31 de octubre de 2012, por lo que no cubre el servicio de almacenamiento en caché de aplicaciones Web en el servicio de aplicaciones de Azure que se introdujo en 2013.

Ejemplo de código

- [Fundamentos de servicio en Azure en la nube](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649). Aplicación de ejemplo que implementa el almacenamiento en caché distribuido. Consulte el blog que lo acompaña [Fundamentos de servicio de nube: conceptos básicos de almacenamiento en caché](https://blogs.msdn.com/b/windowsazure/archive/2013/10/03/cloud-service-fundamentals-caching-basics.aspx).

>[!div class="step-by-step"]
[Anterior](transient-fault-handling.md)
[Siguiente](queue-centric-work-pattern.md)
