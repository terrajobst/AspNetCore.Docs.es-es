---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-partitioning-strategies
title: Estrategias (creación de aplicaciones de nube reales con Azure) de la partición de datos | Documentos de Microsoft
author: MikeWasson
description: Las aplicaciones de nube de creación Real World con libros electrónicos Azure se basa en una presentación desarrollada por Scott Guthrie. Se explican 13 patrones y prácticas recomendadas que puede...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: 513837a7-cfea-4568-a4e9-1f5901245d24
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-partitioning-strategies
msc.type: authoredcontent
ms.openlocfilehash: 9ff7f37a03d8d3dfab50e8007a6645bb0d88f453
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="data-partitioning-strategies-building-real-world-cloud-apps-with-azure"></a>Estrategias (creación de aplicaciones de nube reales con Azure) de la partición de datos
====================
por [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Descarga corregirlo proyecto](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) o [descargar libros electrónicos](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> El **compilar aplicaciones de nube de mundo Real con Azure** libros electrónicos se basa en una presentación desarrollada por Scott Guthrie. Se explican los 13 patrones y prácticas recomendadas que le permitirá tener éxito el desarrollo de aplicaciones web para la nube. Para obtener información acerca de la serie, consulte [el primer capítulo](introduction.md).


Anteriormente, hemos visto lo fácil que es escalar el nivel web de una aplicación en la nube, agregar y quitar servidores web. Pero si todos están cumpliendo el mismo almacén de datos, el cuello de botella de la aplicación pasa de front-end al back-end, y es el más difícil de escalar la capa de datos. En este capítulo, veremos cómo se puede establecer el nivel de datos escalable mediante la partición de datos en varias bases de datos relacionales, o mediante la combinación de almacenamiento de base de datos relacional con otras opciones de almacenamiento de datos.

Configurar un esquema de partición es mejor done la parte delantera de la misma razón que se ha mencionado anteriormente: es muy difícil de modificar su estrategia de almacenamiento de datos cuando una aplicación esté en producción. Si piensa disco duro por adelantado sobre los distintos métodos, no puede tener un "momento de Twitter" cuando la aplicación se bloquea o deja de funcionar durante mucho tiempo mientras reorganizar los datos y código de acceso de datos de la aplicación.

## <a name="the-three-vs-of-data-storage"></a>Lo tres Vs del almacenamiento de datos

Para determinar si necesita una estrategia de particiones y cuáles deben ser, tenga en cuenta tres preguntas sobre los datos:

- ¿Volumen: la cantidad de datos en el que podrá almacenar en última instancia? ¿Dos gigabytes? ¿Unos cientos de gigabytes? ¿Terabytes? ¿Petabytes?
- Progreso: ¿cuál es la tasa de crecen de los datos? ¿Es una aplicación interna que no está generando una gran cantidad de datos? ¿Una aplicación externa que los clientes va a cargar imágenes y vídeos en?
- ¿Diversos: el tipo de datos tendrá que almacenar? ¿Relacionales, imágenes, pares de clave-valor, sociales gráficos?

Si cree que va a tiene una gran cantidad de volumen, progreso o variedad, debe tener en cuenta cuidadosamente qué tipo de esquema de partición mejor habilitará la aplicación escalar de forma eficaz a medida que crece, y para asegurarse de que no se ejecutan en cualquier cuello de botella.

Básicamente, hay tres enfoques para la creación de particiones:

- Particionamiento vertical
- creación de particiones horizontales
- Particionamiento híbrido

## <a name="vertical-partitioning"></a>Particionamiento vertical

Separando en porciones vertical es similar a dividir una tabla por columnas: un conjunto de columnas que se entra en el almacén de datos, y otro conjunto de columnas entra en un almacén de datos diferente.

Por ejemplo, suponga que la aplicación almacena datos acerca de las personas, incluidas las imágenes:

![Tabla de datos](data-partitioning-strategies/_static/image1.png)

Al representar estos datos como una tabla y examine los diferentes tipos de datos, puede ver que las tres columnas de la izquierda tienen datos de cadena que se pueden almacenar eficazmente una base de datos relacional, mientras que las dos columnas de la derecha son básicamente las matrices de bytes que c Articular desde archivos de imagen. Es posible a los datos de archivo de imagen de almacenamiento en una base de datos relacional, y una gran cantidad de personas hacerlo ya que no quieran guardar los datos en el sistema de archivos. Que no tengan un sistema de archivos capaz de almacenar los volúmenes de datos requiere o no es posible que prefiera administrar una copia independiente de seguridad y restaurar sistema. Este enfoque funciona bien para bases de datos local y para pequeñas cantidades de datos en las bases de datos en la nube. En el entorno local, podría ser más fácil simplemente dejar que los DBA ocuparse de todo el contenido.

Pero en una base de datos en la nube, es relativamente costoso almacenamiento y un gran volumen de imágenes podría reducir el tamaño de la base de datos crezca más allá de los límites en el que puede funcionar eficazmente. Puede resolver estos problemas mediante los datos de la partición vertical, lo que significa que elija el almacén de datos más adecuado para cada columna de la tabla de datos. Lo que podría funcionar mejor para este ejemplo consiste en colocar los datos de cadena en una base de datos relacional y las imágenes en almacenamiento de blobs.

![Tabla de datos con particiones verticales](data-partitioning-strategies/_static/image2.png)

Almacenar las imágenes en almacenamiento de blobs en lugar de una base de datos es más práctico en la nube que en un entorno local porque no tiene que preocuparse sobre cómo configurar servidores de archivos o administración de copia de seguridad y restauración de los datos almacenados fuera de la base de datos relacional: todos los que se controla automáticamente por el servicio de almacenamiento de blobs.

Éste es el enfoque de partición se implementa en la aplicación repararlo y se examinará el código para que, en la [capítulo de almacenamiento Blob](unstructured-blob-storage.md). Sin este esquema de partición y suponiendo un tamaño de imagen promedio de 3 megabytes, la aplicación repararlo solo sería capaz de almacenar unos 40 000 tareas antes de alcanzar el tamaño máximo de la base de datos de 150 gigabytes. Después de quitar las imágenes, la base de datos puede almacenar 10 veces el número de tareas; puede ir mucho más tiempo antes de tener que pensar acerca de cómo implementar un esquema de partición horizontal. Y como escala de la aplicación, sus gastos crecen más lentamente porque va la mayor parte de las necesidades de almacenamiento en el almacenamiento de Blob barato.

## <a name="horizontal-partitioning-sharding"></a>Particiones (particionamiento) horizontales

Separando en porciones horizontal es similar a dividir una tabla por filas: pasa a un conjunto de filas en el almacén de datos y otro conjunto de filas entra en un almacén de datos diferente.

Con el mismo conjunto de datos, otra opción sería almacenar distintos intervalos de nombres de clientes en distintas bases de datos.

![Tabla de datos particionada horizontalmente](data-partitioning-strategies/_static/image3.png)

Desea ser sumamente cuidadosos cuando el esquema de particionamiento para asegurarse de que los datos se distribuyen uniformemente con el fin de evitar las zonas activas. Este sencillo ejemplo mediante la primera letra del apellido no cumple con ese requisito, dado que muchas de las personas tienen apellidos que empiezan con ciertos letras comunes. Antes de lo que cabría esperar porque algunas bases de datos obtendría muy grandes, mientras que la mayoría se mantendría pequeña alcanzaba limitaciones de tamaño de tabla.

Un inconveniente de particionamiento horizontal es que podría resultar difícil hacer consultas a través de todos los datos. En este ejemplo, una consulta que extraer de hasta 26 diferentes bases de datos para obtener todos los datos almacenados por la aplicación.

## <a name="hybrid-partitioning"></a>Particionamiento híbrido

Puede combinar particiones horizontales y verticales. Por ejemplo, podría almacenar las imágenes en almacenamiento de blobs y dividir horizontalmente los datos de cadena en los datos de ejemplo.

![Híbrido de tabla de datos con particiones](data-partitioning-strategies/_static/image4.png)

## <a name="partitioning-a-production-application"></a>Creación de particiones de una aplicación de producción

Conceptualmente es fácil ver cómo funcionaría un esquema de partición, pero cualquier esquema de partición aumenta la complejidad del código y presenta muchas complicaciones nuevo que tiene que tratar con. Si va a mover las imágenes en el almacenamiento de blobs, ¿qué hacer cuando el servicio de almacenamiento está funcionando? ¿Cómo controlar los blob seguridad? ¿Qué ocurre si el almacenamiento de blobs y de base de datos dejan de estar sincronizado? ¿Si le particionamiento, cómo se controlarán consultar en todas las bases de datos?

Las complicaciones son fáciles de administrar, siempre y cuando se piensa para ellos antes de ir a producción. Muchas personas que no desea que tenían más adelante. Por término medio nuestro equipo de equipo de asesoramiento al cliente (CAT) obtiene genera un error de llamadas de teléfono sobre una vez al mes de los clientes cuyas aplicaciones están tardando de forma muy grande y no hacen esta fase de preparación. Y dicen algo parecido a: "Ayuda". Colocar todo en un almacén de datos único, y en 45 días voy a quedarse sin espacio en el mismo!" Y si tiene una gran cantidad de lógica de negocios que integran el acceso a su almacén de datos y tiene clientes que usen la aplicación, no hay ningún tiempo buena a dejan de funcionar durante un mismo día durante la migración. Se terminará recorrer herculean esfuerzos para ayudar a la partición de cliente sus datos sobre la marcha sin perder tiempo. Resulta muy interesante y muy terror y algo no desea que estén implicados en si se pueden evitar! Pensar en esto por adelantado e integrar en la aplicación hará que su vida mucho más fácil si la aplicación aumenta más adelante.

## <a name="summary"></a>Resumen

Un esquema de partición efectivo puede permitir que la aplicación en la nube escalar a petabytes de datos en la nube sin cuellos de botella. Y no tenga que pagar por adelantado para máquinas masivas o una amplia infraestructura tal y como lo haría si estuviera ejecutando la aplicación en un centro de datos local. En la nube, puede incrementalmente puede agregar capacidad a medida que lo necesite, y cobran solo para lo que está utilizando cuando se utiliza.

En el [siguiente capítulo](unstructured-blob-storage.md) vamos a ver cómo la aplicación repararlo implementa el particionamiento vertical mediante el almacenamiento de imágenes en almacenamiento de blobs.

## <a name="resources"></a>Recursos

Para obtener más información sobre las estrategias de partición, vea los siguientes recursos.

Documentación:

- [Procedimientos recomendados para el diseño de servicios a gran escala en los servicios de nube de Azure Windows](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). Notas del producto, Mark Simms y Michael Thomassy.
- [Microsoft patrones y prácticas: patrones de diseño en la nube](https://msdn.microsoft.com/library/dn568099.aspx). Consulte las instrucciones de creación de particiones de datos, el patrón de particionamiento.

Vídeos:

- [FailSafe: Creación de servicios en la nube escalables y resistentes](https://channel9.msdn.com/Series/FailSafe). Serie de nueve partes por Ulrich Homann y Marc Mercuri, Mark Simms. Presenta los conceptos y principios de la arquitectura de una manera muy interesante y accesible, con casos extraídos de la experiencia del equipo de asesoramiento al cliente (CAT) de Microsoft con clientes reales. Vea la explicación de partición en episodio 7.
- [Creación Big: Lecciones aprendidas de clientes de Windows Azure, parte I](https://channel9.msdn.com/Events/Build/2012/3-029). Mark Simms se describen los esquemas de partición, estrategias de particionamiento, cómo implementar el particionamiento y federaciones de base de datos de SQL, a partir de 19:49. Similar a la serie Failsafe pero queda procedimientos con mayor detalle.

Código de ejemplo:

- [Fundamentos de servicio en Windows Azure en la nube](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649). Aplicación de ejemplo que incluye una base de datos particionada. Para obtener una descripción del esquema de particionamiento implementado, vea [DAL: particionamiento de RDBMS](https://blogs.msdn.com/b/windowsazure/archive/2013/09/05/dal-sharding-of-rdbms.aspx) en el blog de Windows Azure.

> [!div class="step-by-step"]
> [Anterior](data-storage-options.md)
> [Siguiente](unstructured-blob-storage.md)
