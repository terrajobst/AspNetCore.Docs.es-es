---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-partitioning-strategies
title: (Crear aplicaciones de nube reales con Azure) de las estrategias de partición de datos | Microsoft Docs
author: MikeWasson
description: Building Real World Cloud Apps with e-book de Azure se basa en una presentación desarrollada por Scott Guthrie. Explican el 13 de patrones y prácticas que puede...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: 513837a7-cfea-4568-a4e9-1f5901245d24
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-partitioning-strategies
msc.type: authoredcontent
ms.openlocfilehash: 07a80767ca2def26c0252037a3b8b990abcbe1b3
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/04/2018
ms.locfileid: "48578489"
---
<a name="data-partitioning-strategies-building-real-world-cloud-apps-with-azure"></a>(Crear aplicaciones de nube reales con Azure) de las estrategias de partición de datos
====================
por [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson]((https://twitter.com/RickAndMSFT)), [Tom Dykstra](https://github.com/tdykstra)

[Descargar proyecto corregirlo](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) o [descargar libro electrónico](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> El **Building Real World Cloud Apps with Azure** eBook se basa en una presentación desarrollada por Scott Guthrie. Se explican el 13 patrones y prácticas que pueden ayudarle a tener éxito el desarrollo de aplicaciones web para la nube. Para obtener información acerca de la serie, vea [el primer capítulo](introduction.md).


Anteriormente hemos visto lo fácil que es escalar el nivel web de una aplicación en la nube, mediante la adición y eliminación de servidores web. Pero si todo está alcanzando el mismo almacén de datos, el cuello de botella de la aplicación se mueve desde el front-end al back-end, y es el más difícil de escalar la capa de datos. En este capítulo, veremos cómo puede hacer la capa de datos escalable mediante la partición de datos en varias bases de datos relacionales, o mediante la combinación de almacenamiento de base de datos relacional con otras opciones de almacenamiento de datos.

La configuración de un esquema de partición es mejor done anticipado por la misma razón que se ha mencionado anteriormente: resulta muy difícil de cambiar su estrategia de almacenamiento de datos después de una aplicación en producción. Si piensa en disco duro por adelantado enfoques diferentes, puede evitar un "momento Twitter" cuando la aplicación se bloquea o deja de funcionar durante mucho tiempo mientras reorganizar los datos y código de acceso a datos de la aplicación.

## <a name="the-three-vs-of-data-storage"></a>Lo tres Vs del almacenamiento de datos

Con el fin de determinar si necesita una estrategia de partición y lo que debe ser, tenga en cuenta tres preguntas acerca de los datos:

- ¿En última instancia, almacenar el volumen: la cantidad de datos que podrá? ¿Algunos gigabytes? ¿Unos cientos de gigabytes? ¿Terabytes? ¿Petabytes?
- Velocidad: ¿qué es la tasa de crecen de los datos? ¿Es una aplicación interna que no está generando una gran cantidad de datos? ¿Una aplicación externa que los clientes va a cargar las imágenes y vídeos en?
- ¿Variedad, el tipo de datos tendrá que almacenar? ¿Relacionales, imágenes, pares clave-valor, gráficos sociales?

Si piensa que vas a tiene una gran cantidad de volumen, velocidad o variedad, deberá considerar cuidadosamente qué tipo de esquema de partición mejor permitirá a la aplicación escalar de manera eficiente y eficaz a medida que crece, y para asegurarse de no se ejecuta en los cuellos de botella.

Básicamente, hay tres enfoques para la creación de particiones:

- Creación de particiones verticales
- Creación de particiones horizontales
- Particionamiento híbrido

## <a name="vertical-partitioning"></a>Creación de particiones verticales

Separando en porciones de vertical es similar a una tabla de la división por columnas: un conjunto de columnas entra en un almacén de datos y otro conjunto de columnas entra en un almacén de datos diferente.

Por ejemplo, suponga que mi aplicación almacena datos sobre las personas, incluidas las imágenes:

![Tabla de datos](data-partitioning-strategies/_static/image1.png)

Al representar estos datos como una tabla y examine los diferentes tipos de datos, puede ver que las tres columnas de la izquierda tienen datos de cadena que se pueden almacenar eficazmente mediante una base de datos relacional, mientras que las dos columnas de la derecha son básicamente las matrices de bytes c ome desde archivos de imagen. Es posible a los datos de archivo de imagen de almacenamiento en una base de datos relacional, y muchas personas hacerlo porque no quieren guardar los datos en el sistema de archivos. Que no tengan un sistema de archivos capaz de almacenar los volúmenes de datos necesarios o es posible que no desean administrar una copia independiente de seguridad y restauración del sistema. Este enfoque funciona bien para las bases de datos de forma local y para pequeñas cantidades de datos en las bases de datos en la nube. En el entorno local, podría ser más fácil simplemente dejar que el DBA se encargue de todo el contenido.

Pero en una base de datos en la nube, es relativamente costoso almacenamiento y un gran volumen de las imágenes podría hacer que el tamaño de la base de datos crezca más allá de los límites en el que puede funcionar eficazmente. Puede solucionar estos problemas mediante la partición de los datos verticalmente, lo que significa que elija el almacén de datos más adecuado para cada columna en la tabla de datos. Lo que podría funcionar mejor para este ejemplo consiste en colocar los datos de cadena en una base de datos relacional y las imágenes en Blob storage.

![Tabla de datos con particiones verticales](data-partitioning-strategies/_static/image2.png)

Almacenar las imágenes en Blob storage en lugar de una base de datos es más práctico en la nube que en un entorno local porque no tiene que preocuparse por servidores de archivos de configuración o administración de copia de seguridad y restauración de los datos almacenados fuera de la base de datos relacional: todas que se controla por usted automáticamente por el servicio de almacenamiento de blobs.

Este es el enfoque de partición se implementa en la aplicación Fix It y veremos el código para que, en el [capítulo del almacenamiento de blobs](unstructured-blob-storage.md). Sin este esquema de partición y suponiendo un tamaño de imagen promedio de 3 megabytes, la aplicación Fix It solo sería capaz de almacenar aproximadamente 40 000 tareas antes de alcanzar el tamaño de la base de datos máximo de 150 gigabytes. Después de quitar las imágenes, la base de datos puede almacenar 10 veces más tantas tareas; puede ir mucho más tiempo antes de tener que pensar acerca de cómo implementar un esquema de partición horizontal. Y, a medida que se escala la aplicación, los gastos crecen más lentamente porque se pasará la mayor parte de las necesidades de almacenamiento al almacenamiento de blobs muy económico.

## <a name="horizontal-partitioning-sharding"></a>Horizontal (particionamiento) de la creación de particiones

Separando en porciones de horizontal es como una tabla de la división por filas: un conjunto de filas entra en un almacén de datos y otro conjunto de filas entra en un almacén de datos diferente.

Con el mismo conjunto de datos, otra opción sería almacenar distintos intervalos de nombres de los clientes en diferentes bases de datos.

![Tabla de datos particionada horizontalmente](data-partitioning-strategies/_static/image3.png)

Desea tener gran cuidado respecto de su esquema de particionamiento para asegurarse de que los datos se distribuyen uniformemente para evitar las zonas activas. Este sencillo ejemplo utilizando la primera letra del apellido no cumple este requisito, ya que muchas personas tienen los apellidos que comienzan con determinadas letras comunes. Antes de lo esperado, ya que obtención grandes algunas bases de datos mientras se mantendría pequeño la mayor parte se alcanzan las limitaciones de tamaño de tabla.

Un inconveniente de particionamiento horizontal es que puede ser difícil de hacer consultas en todos los datos. En este ejemplo, una consulta tendría que dibujar de hasta 26 diferentes bases de datos para obtener todos los datos almacenados por la aplicación.

## <a name="hybrid-partitioning"></a>Particionamiento híbrido

Puede combinar las particiones verticales y horizontales. Por ejemplo, podría almacenar las imágenes en Blob storage y particionar horizontalmente los datos de cadena en los datos de ejemplo.

![Híbrido de la tabla de datos con particiones](data-partitioning-strategies/_static/image4.png)

## <a name="partitioning-a-production-application"></a>Creación de particiones de una aplicación de producción

Conceptualmente es fácil ver cómo funcionaría un esquema de partición, pero cualquier esquema de partición aumenta la complejidad del código e introduce una complicación muchos nuevos que se debe tratar con. Si va a mover las imágenes al almacenamiento de blobs, ¿qué hacer cuando el servicio de almacenamiento está funcionando? ¿Cómo controlar la seguridad de blob? ¿Qué ocurre si la base de datos y blob storage dejan de estar sincronizado? Si le particionamiento, ¿cómo manejará consultar en todas las bases de datos?

Las complicaciones son fáciles de administrar, siempre y cuando está planeando para ellos antes de ir a producción. Muchas personas que no hizo que desea que tenían más adelante. Por término medio nuestro equipo de equipo de asesoramiento al cliente (CAT) obtiene asustadas llamadas telefónicas sobre una vez al mes de los clientes cuyas aplicaciones están tardando de forma muy grande y no hacen esta planificación. Y dicen que algo como: "Ayuda! Colocar todo en un único almacén de datos, y en 45 días, voy a quedarse sin espacio en él!" Y si tiene una gran cantidad de lógica de negocios integrada en el acceso a su almacén de datos y tiene clientes que usan la aplicación, no hay ningún tiempo buena deje de funcionar durante un día durante la migración. Terminamos repasar herculean esfuerzos para ayudar a la partición de cliente sus datos sobre la marcha sin perder tiempo. Es muy emocionante y muy miedo y no algo desea estar implicada en si se puede evitar! Pensando en esto por adelantado y la integración en la aplicación hará que su vida mucho más fácil si la aplicación crece más adelante.

## <a name="summary"></a>Resumen

Un esquema de partición efectiva puede permitir que la aplicación en la nube escalar a petabytes de datos en la nube sin cuellos de botella. Y no tiene que pagar por adelantado para grandes máquinas o una amplia infraestructura lo haría si estuviera ejecutando la aplicación en un centro de datos local. En la nube, puede incrementalmente puede agregar capacidad a medida que lo necesite y solo está pagando para lo que está utilizando cuando se use.

En el [siguiente capítulo](unstructured-blob-storage.md) veremos cómo la aplicación Fix It implementa las particiones verticales almacenando las imágenes en Blob storage.

## <a name="resources"></a>Recursos

Para obtener más información acerca de estrategias de partición, vea los siguientes recursos.

Documentación:

- [Procedimientos recomendados para el diseño de servicios a gran escala en Microsoft Azure Cloud Services](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). Notas del producto, Mark Simms y Michael Thomassy.
- [Microsoft Patterns and Practices: patrones de diseño en la nube](https://msdn.microsoft.com/library/dn568099.aspx). Consulte las instrucciones de creación de particiones de datos, el patrón de particionamiento.

Vídeos:

- [FailSafe: Creación de servicios de nube escalables, resistentes](https://channel9.msdn.com/Series/FailSafe). Serie de nueve partes, Ulrich Homann, Marc Mercuri y Mark Simms. Presenta conceptos y principios de la arquitectura de una manera muy accesible e interesante, con casos procedentes de la experiencia del equipo de asesoramiento al cliente (CAT) de Microsoft con clientes reales. Vea la explicación de creación de particiones en el episodio 7.
- [Creación Big: Lecciones aprendidas de los clientes de Windows Azure: parte I](https://channel9.msdn.com/Events/Build/2012/3-029). Mark Simms se describen los esquemas de partición, estrategias de particionamiento, cómo implementar el particionamiento y federaciones de base de datos de SQL, empezando en 19:49. Similar a la serie Failsafe pero entran en obtener más información sobre procedimientos.

Código de ejemplo:

- [En la nube Fundamentos de servicio en Windows Azure](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649). Aplicación de ejemplo que incluye una base de datos particionada. Para obtener una descripción del esquema de particionamiento implementado, vea [DAL: particionamiento de RDBMS](https://blogs.msdn.com/b/windowsazure/archive/2013/09/05/dal-sharding-of-rdbms.aspx) en el blog de Windows Azure.

> [!div class="step-by-step"]
> [Anterior](data-storage-options.md)
> [Siguiente](unstructured-blob-storage.md)
