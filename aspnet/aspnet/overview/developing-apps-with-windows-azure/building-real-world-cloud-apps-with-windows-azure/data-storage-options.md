---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options
title: Opciones de almacenamiento de datos (creación de aplicaciones de nube reales con Azure) | Microsoft Docs
author: MikeWasson
description: Building Real World Cloud Apps with e-book de Azure se basa en una presentación desarrollada por Scott Guthrie. Explican el 13 de patrones y prácticas que puede...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: e51fcecb-cb33-4f9e-8428-6d2b3d0fe1bf
ms.technology: ''
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options
msc.type: authoredcontent
ms.openlocfilehash: 43e7a03f3bebcca820452ea10e13459d8d275b5a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37363512"
---
<a name="data-storage-options-building-real-world-cloud-apps-with-azure"></a>Opciones de almacenamiento de datos (creación de aplicaciones de nube reales con Azure)
====================
por [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Descargar proyecto corregirlo](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) o [descargar libro electrónico](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> El **Building Real World Cloud Apps with Azure** eBook se basa en una presentación desarrollada por Scott Guthrie. Se explican el 13 patrones y prácticas que pueden ayudarle a tener éxito el desarrollo de aplicaciones web para la nube. Para obtener información sobre el libro electrónico, consulte [el primer capítulo](introduction.md).


La mayoría de los usuarios está acostumbrados bases de datos relacionales y tienden a pasar por alto otras opciones de almacenamiento de datos cuando diseña una aplicación de nube. El resultado puede ser aún peor, gastos alta o rendimiento no óptimo porque [NoSQL](http://en.wikipedia.org/wiki/NoSQL) bases de datos (no relacionales) pueden controlar algunas tareas de forma más eficaz que las bases de datos relacionales. Cuando los clientes pedirnos ayuda para resolver un problema de almacenamiento de datos críticos, es a menudo porque tienen una base de datos relacional en una de las opciones de NoSQL habría funcionado mejor. En esas situaciones el cliente habría sido preferible si hubiera implementado la solución NoSQL antes de implementar la aplicación en producción.

Por otro lado, también sería un error suponer que una base de datos NoSQL puede hacer todo lo bueno o lo bastante bien. No hay ninguna opción de administración de datos mejor único para todas las tareas de almacenamiento de datos; las soluciones de administración de datos diferentes están optimizadas para diferentes tareas. La mayoría de las aplicaciones de nube del mundo real tienen una gran variedad de requisitos de almacenamiento de datos y a menudo se atienden mejor mediante una combinación de varias soluciones de almacenamiento de datos.

El propósito de este capítulo es proporcionarle un sentido más amplio de las opciones de almacenamiento de datos disponibles en una aplicación en la nube y algunas instrucciones básicas sobre cómo elegir las que se adapten a su escenario. Conviene tener en cuenta las opciones disponibles para usted y pensar sobre sus puntos fuertes y débiles antes de desarrollar una aplicación. Cambiar opciones de almacenamiento de datos en una aplicación de producción puede ser muy difícil, como tener que cambiar un motor jet mientras el plano está en vuelo.

## <a name="data-storage-options-on-azure"></a>Opciones de almacenamiento de datos en Azure

La nube facilita relativamente fácil de usar una variedad de relacionales y almacenes de datos NoSQL. Estas son algunas de las plataformas de almacenamiento de datos que puede usar en Azure.

![](data-storage-options/_static/image1.png)

La tabla muestran los cuatro tipos de bases de datos NoSQL:

- [Las bases de datos de clave-valor](https://msdn.microsoft.com/library/dn313285.aspx#sec7) almacenar un objeto serializado individual para cada valor de clave. Son buenas para almacenar grandes volúmenes de datos donde desea obtener un elemento para un determinado valor de clave y no tiene para consultar en función de otras propiedades del elemento.

    [Almacenamiento de blobs de Azure](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/) es una base de datos de pares clave-valor que funciona como el almacenamiento de archivos en la nube, con valores de clave que se corresponden con los nombres de archivo y carpeta. Recuperar un archivo por su carpeta y el nombre de archivo, no por buscando valores en el contenido del archivo.

    [Azure Table storage](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/) también es una base de datos de clave/valor. Cada valor se denomina un *entidad* (similar a una fila, identificada por una clave de partición y clave de fila) y contiene varias *propiedades* (similar a las columnas, pero no todas las entidades de una tabla deben compartir el mismo columnas). Realizar consultas en columnas distintas de la clave es extremadamente ineficaz y deben evitarse. Por ejemplo, puede almacenar datos de perfil de usuario, con una partición que almacena información acerca de un solo usuario. Puede almacenar datos como nombre de usuario, hash de contraseña, fecha de nacimiento y así sucesivamente, en propiedades distintas de una entidad o entidades independientes en la misma partición. Pero no desea consultar para todos los usuarios con un determinado intervalo de fechas de nacimiento y no se puede ejecutar una consulta de combinación entre la tabla de perfiles y la otra tabla. Almacenamiento de tablas es más escalable y menos costoso que una base de datos relacional, pero no habilita las consultas complejas o combinaciones.
- [Documentdatabases](https://msdn.microsoft.com/library/dn313285.aspx#sec8) son bases de datos de clave/valor en el que los valores son *documentos*. "Documento" no se usa en el sentido de un documento de Word o Excel, pero significa que una colección de campos con nombre y valores, que podría ser un documento secundario. Por ejemplo, en una tabla de historial de pedidos podría tener un documento de pedido número de pedido, fecha de pedido y los campos del cliente; y el campo cliente podría tener campos de nombre y dirección. La base de datos codifica los datos de campo en un formato como XML, YAML, JSON o BSON; o bien, puede usar texto sin formato. Una característica que establece las bases de datos de documentos además de las bases de datos de clave/valor es la capacidad para consultar en los campos que no son de clave y definir índices secundarios para simplificar las consultas más eficaz. Esta capacidad hace que una base de datos del documento sea más adecuado para las aplicaciones que necesitan recuperar datos en función de criterios más complejos que el valor de la clave del documento. Por ejemplo, en una base de datos del documento de historial de pedidos de ventas puede realizar consultas en diversos campos, como el Id. de producto, Id. de cliente, nombre del cliente y así sucesivamente. [MongoDB](http://www.mongodb.org/) es una base de datos de documentos populares.
- [Las bases de datos de la familia de columnas](https://msdn.microsoft.com/library/dn313285.aspx#sec9) son los almacenes de datos que le permiten que estructurar el almacenamiento de datos en colecciones de columnas relacionadas denominadas familias de columnas de clave/valor. Por ejemplo, una base de datos del censo podría tener un grupo de columnas de nombre de una persona (primero, segundo nombre, apellidos), un grupo de direcciones de la persona y un grupo de información de perfil de la persona (fecha de nacimiento, sexo, etcetera.). La base de datos, a continuación, puede almacenar cada familia de columnas en una partición independiente al tiempo que mantiene todos los datos de una persona relacionados con la misma clave. A continuación, puede leer toda la información de perfil sin tener que leer toda la información de nombre y dirección. [Cassandra](http://cassandra.apache.org/) es una conocida base de datos de la familia de columnas.
- [Las bases de datos del gráfico](https://msdn.microsoft.com/library/dn313285.aspx#sec10) almacenar información como una colección de objetos y relaciones. El propósito de una base de datos de gráfico es permitir que una aplicación realizar consultas que recorran la red de los objetos y las relaciones entre ellos de manera eficaz. Por ejemplo, los objetos podrían ser los empleados en una base de datos de recursos humanos, y es posible que desee para facilitar las consultas, como "buscan todos los empleados que trabajan directa o indirectamente para Scott." [Neo4j](http://www.neo4j.org/) es una base de datos de gráfico más populares.

En comparación con las bases de datos relacionales, las opciones de NoSQL ofrecen mayor escalabilidad y rentabilidad para el almacenamiento y análisis de datos no estructurados. El inconveniente es que no ofrecen la queryability enriquecido y funcionalidades de integridad de datos sólido de bases de datos relacionales. NoSQL podría funcionar bien para los datos de registro IIS, lo que implica el gran volumen sin necesidad de consultas de combinación. NoSQL no funcionaría tan bien para banca transacciones, que requiere integridad de los datos absoluta e implica muchas relaciones con otros datos relacionados con la cuenta.

También hay una categoría más reciente de la plataforma de base de datos denominada [NewSQL](http://en.wikipedia.org/wiki/NewSQL) que combina la escalabilidad de una base de datos NoSQL con la queryability y la integridad transaccional de una base de datos relacional. Las bases de datos NewSQL están diseñados para almacenamiento distribuido y procesamiento de consultas, que a menudo es difícil de implementar en las bases de datos "OldSQL". [NuoDB](http://www.nuodb.com/) es un ejemplo de una base de datos NewSQL que puede usarse en Azure.

<a id="hadoop"></a>
## <a name="hadoop-and-mapreduce"></a>Hadoop y MapReduce

Los grandes volúmenes de datos que se pueden almacenar en bases de datos NoSQL pueden resultar difíciles analizar de forma eficaz de manera oportuna. Hacer que puede usar un marco como [Hadoop](http://hadoop.apache.org/) que implementa [MapReduce](http://en.wikipedia.org/wiki/MapReduce) funcionalidad. Esencialmente hace que un proceso de MapReduce es el siguiente:

- Limitar el tamaño de los datos que deben procesarse seleccionando fuera de los datos del almacén solo los datos realmente se necesita para analizar. Por ejemplo, desea conocer la composición de su base por año de nacimiento del usuario para poder seleccionar solo los años de nacimiento fuera de su almacén de datos de perfil de usuario.
- Desglosar los datos en partes y envíelos a diferentes equipos para su procesamiento. El equipo A calcula el número de personas con las fechas de 1950 1959, el equipo B hace 1969 de 1960, etcetera. Se llama a este grupo de equipos una *clúster de Hadoop*.
- Colocar los resultados de cada parte de nuevo después de que se realiza el procesamiento en las partes. Ahora tiene una lista relativamente corta de cuántas personas de cada año de nacimiento y la tarea de calcular los porcentajes en la lista global es fácil de administrar.

En Azure, [HDInsight](https://azure.microsoft.com/services/hdinsight/) permite procesar, analizar y obtener nuevas perspectivas de big data con la potencia de Hadoop. Por ejemplo, podría utilizarlo para analizar registros de servidor web:

- Habilitar el registro de servidor web para la cuenta de almacenamiento. Esto configura Azure para escribir registros en el servicio Blob para todas las solicitudes HTTP a la aplicación. Blob Service es básicamente el almacenamiento de archivos en la nube, y se integra perfectamente con HDInsight. 

    ![Registros al almacenamiento de blobs](data-storage-options/_static/image2.png)
- A medida que la aplicación de tráfico, registros de IIS del servidor web se escriben en el almacenamiento de blobs. 

    ![Registros de servidor Web](data-storage-options/_static/image3.png)
- En el portal, haga clic en **New** - **Data Services** - **HDInsight** - **creación rápida**, y especifique un nombre de clúster de HDInsight, el tamaño de clúster (número de nodos de datos del clúster de HDInsight) y un nombre de usuario y una contraseña para el clúster de HDInsight. 

    ![HDInsight](data-storage-options/_static/image4.png)

Ahora puede configurar trabajos de MapReduce para analizar los registros y obtenga respuestas a preguntas como:

- ¿Qué horas del día obtener mi aplicación el tráfico o más?
- ¿Qué países está mi tráfico procedente de?
- ¿Qué es los ingresos vecindario promedio de las áreas que proviene de mi tráfico. (Hay un conjunto de datos público que ofrece resultados del vecindario por dirección IP, y puede hacer coincidir con la dirección IP en los registros del servidor web).
- ¿Cómo se correlacionan ingresos vecindario a páginas específicas o productos en el sitio?

A continuación, podría usar las respuestas a preguntas como estas probable que compren un producto determinado o en función de la probabilidad de en que un cliente estaría interesado los anuncios de destino.

Como se explica en el [automatizar todo capítulo](automate-everything.md), se puede automatizar la mayoría de las funciones que puede hacer en el portal y que incluye la configuración y ejecución de los trabajos de análisis de HDInsight. Una secuencia de comandos típica de HDInsight puede contener los siguientes pasos:

- Aprovisionar un clúster de HDInsight y vincularla a su cuenta de almacenamiento para la entrada de almacenamiento de blobs.
- Cargue los archivos ejecutables de trabajo de MapReduce (archivos .jar o .exe) en el clúster de HDInsight.
- Enviar un MapReduce que almacena los datos de salida en almacenamiento de blobs.
- Espere a que finalice el trabajo.
- Elimine el clúster de HDInsight.
- Acceder a la salida de Blob storage.

Al ejecutar un script que haga todo esto, se minimiza la cantidad de tiempo que se ha aprovisionado el clúster de HDInsight, lo que minimiza los costos.

<a id="paasiaas"></a>
## <a name="platform-as-a-service-paas-versus-infrastructure-as-a-service-iaas"></a>Plataforma como servicio (PaaS) frente a la infraestructura como servicio (IaaS)

Las opciones de almacenamiento de datos enumeradas anteriormente incluyen la plataforma como-servicio (PaaS) y las soluciones de infraestructura como-servicio (IaaS). PaaS significa que se administra la infraestructura de hardware y software y simplemente usar el servicio. SQL Database es una característica de PaaS de Azure. Puede formular las bases de datos, y en segundo plano Azure establece y configura las máquinas virtuales y configura las bases de datos en ellos. No tiene acceso directo a las máquinas virtuales y no tiene que administrarlas. IaaS significa que configure, configurar y administrar máquinas virtuales que se ejecutan en nuestra infraestructura de centro de datos, y coloca todo lo desee en ellos. Proporcionamos una galería de imágenes de máquina virtual configuradas previamente para las configuraciones de máquina virtual común. Por ejemplo, puede instalar imágenes de máquinas virtuales preconfiguradas para Windows Server 2008, Windows Server 2012, BizTalk Server, Oracle WebLogic Server, Oracle Database, etcetera.

Las soluciones de datos de PaaS que ofrece Azure incluyen:

- Azure SQL Database (conocido anteriormente como SQL Azure). En la nube base de datos relacional basado en SQL Server.
- Azure Table storage. Un clave y valor base de datos NoSQL.
- Almacenamiento de blobs de Azure. Almacenamiento de archivos en la nube.

IaaS, puede ejecutar cualquier cosa que puede cargar en una máquina virtual, por ejemplo:

- Bases de datos relacionales como SQL Server, Oracle, MySQL, SQL Compact, SQLite o Postgres.
- Almacenes de datos de clave/valor como Memcached, Redis, Cassandra y Riak.
- Almacenes de datos de columna como HBase.
- Bases de datos documentales, como MongoDB, RavenDB y CouchDB.
- Bases de datos de gráficos como Neo4j.

![Opciones de almacenamiento de datos en Azure](data-storage-options/_static/image5.png)

La opción de IaaS ofrece opciones de almacenamiento de datos prácticamente ilimitado y muchos de ellos son especialmente fáciles de usar porque se pueden crear máquinas virtuales con imágenes preconfiguradas. Por ejemplo, en el portal de administración vaya a **máquinas virtuales**, haga clic en el **imágenes** ficha y haga clic en **examinar VM Depot**.

![Examinar VM Depot](data-storage-options/_static/image6.png)

A continuación, verá una lista de [centenares de imágenes de máquina virtual preconfiguradas](http://www.hanselman.com/blog/Over400VirtualMachineImagesOfOpenSourceSoftwareStacksInTheVMDepotAzureGallery.aspx), y puede crear una máquina virtual desde una imagen que tiene preinstalado un sistema de administración de base de datos, como MongoDB, Neo4J, Redis, Cassandra, CouchDB:

![MongoDB en VM Depot](data-storage-options/_static/image7.png)

Azure ofrece opciones de almacenamiento de datos de IaaS tan sencillas como sea posible, pero las ofertas de PaaS tienen muchas ventajas que los hacen más rentable y práctico para muchos escenarios:

- No es necesario que crear máquinas virtuales, simplemente use el portal o una secuencia de comandos para configurar un almacén de datos. Si desea que un almacén de datos de 200 terabytes, puede hacer clic en un botón o ejecutar un comando y, en segundos está listo para su uso.
- No tiene que administre o mantenga las máquinas virtuales que usa el servicio; Microsoft encarga de ello automáticamente.-no tiene que preocuparse sobre cómo configurar la infraestructura de escalado o alta disponibilidad; Microsoft se encarga de todo lo que para usted.
- No tienes que comprar licencias; las cuotas de licencia se incluyen en las tarifas de servicio.
- Solo paga por lo que usa.

Opciones de almacenamiento de datos de PaaS en Azure incluyen las ofertas por proveedores de terceros. Por ejemplo, puede elegir el [complemento MongoLab](https://azure.microsoft.com/documentation/articles/store-mongolab-web-sites-dotnet-store-data-mongodb/) desde el Store de Azure para aprovisionar una base de datos de MongoDB como servicio.

## <a name="choosing-a-data-storage-option"></a>Elegir una opción de almacenamiento de datos

No hay un enfoque es adecuado para todos los escenarios. Si alguien dice que esta tecnología es la respuesta, lo primero que se pregunte es "¿Qué es la pregunta?", porque se optimizan diferentes soluciones para cosas diferentes. Hay ventajas definitivas en el modelo relacional; Por eso ha sido alrededor para siempre. Pero también son lados de abajo a SQL que pueden tratarse con una solución NoSQL.

A menudo se ve mejor trabajo es un enfoque composicional, donde usa SQL y NoSQL en una única solución. Incluso cuando la gente piensa que va a adoptar NoSQL, si obtiene los detalles en lo que hacen a menudo find que están usando varios marcos diferentes de NoSQL: están usando [CouchDB](http://wiki.apache.org/couchdb/Introduction), y [Redis](http://redis.io/)y [ Riak](http://basho.com/riak/) para cosas diferentes. Incluso Facebook, que usa NoSQL ampliamente, usa diferentes marcos de NoSQL para las distintas partes del servicio. La flexibilidad de mezclar y combinar los enfoques de almacenamiento de datos es una de las cosas bueno de la nube, porque es fácil de usar varias soluciones de datos e integrarlos en una sola aplicación.

Estas son algunas preguntas que pensar cuando se elige un enfoque:

| Semántica de datos | -¿Qué es el núcleo datos y almacenamiento de acceso a datos semántico (¿está almacenando datos relacionales o no estructurados)? Datos no estructurados como archivos multimedia se adapta mejor a almacenamiento de blob; una colección de los datos relacionados, como productos, inventarios, proveedores, pedidos de clientes, etc., se adapta mejor a una base de datos relacional. |
| --- | --- |
| Compatibilidad con consultas | -¿Resulta sencillo para consultar los datos? -¿Qué tipos de preguntas que pueden ser eficaz más frecuentes? Almacenes de datos de clave/valor son muy buenas en la introducción de una sola fila tiene un valor de clave pero no tan bueno para consultas complejas. Para un almacén de datos de perfil de usuario que siempre Obtenga los datos para un usuario determinado, un almacén de datos de clave-valor podría funcionar bien; para un catálogo de productos en la que desee obtener diferentes agrupaciones según distintos atributos del producto base de datos relacional podría funcionar mejor. Bases de datos NoSQL pueden almacenar grandes volúmenes de datos de forma eficaz, pero tiene que estructurar la base de datos en torno a cómo la aplicación consulta los datos y esto hace que las consultas ad hoc que sea más difícil de hacer. Con una base de datos relacional, puede crear prácticamente cualquier tipo de consulta. |
| Proyección funcional | -Puede preguntas, agregaciones, etc., ser ejecutadas en el servidor? Si se ejecuta SELECT COUNT (\*) de una tabla de SQL, realizará muy eficazmente todo el trabajo en el servidor y devolver el número que estoy buscando. Si es necesario el mismo cálculo de un almacén de datos NoSQL que no es compatible con agregaciones, esto es una "unbounded consulta" ineficaz y probablemente el tiempo de espera. Incluso si la consulta se realiza correctamente debe recuperar todos los datos del servidor al cliente y contar las filas en el cliente. -¿Qué idiomas o tipos de expresiones se pueden usar? ¿Puedo usar SQL con una base de datos relacional. Con algunas bases de datos NoSQL como Azure Table storage, vamos a usar [OData](http://www.odata.org/), y todo lo que puedo hacer es filtrar por clave principal y obtener las proyecciones (seleccionar un subconjunto de los campos disponibles). |
| Facilidad de escalabilidad | ¿-La frecuencia y el cantidad se necesitan los datos a escala? ¿-Implementa la plataforma de forma nativa escalada? -¿Resulta sencillo agregar o quitar capacidad (rendimiento y tamaño)? Las tablas y bases de datos relacionales no se particionan automáticamente para que sean escalables, por lo que son difíciles de escalar más allá de ciertas limitaciones. Almacenes de datos NoSQL como Azure Table storage de partición inherentemente todo y hay casi ningún límite a la adición de particiones. Puede escalar fácilmente Table Storage hasta 200 terabytes, pero el tamaño máximo de la base de datos de Azure SQL Database es de 500 gigabytes. Puede escalar datos relacionales mediante la creación de particiones en varias bases de datos, pero la configuración de una aplicación para admitir ese modelo implica una gran cantidad de trabajo de programación. |
| Instrumentación y la facilidad de uso | ¿-Fácil es la plataforma para instrumentar, supervisar y administrar? Debe mantenerse informado sobre el estado y el rendimiento de su almacén de datos, por lo que necesita saber por adelantado qué métricas una plataforma le ofrece de forma gratuita y qué se debe desarrollar usted mismo. |
| Operaciones | ¿-Fácil es la plataforma para implementar y ejecutar en Azure? ¿PaaS? ¿IaaS? ¿Linux? Son fáciles de configurar en Azure Table Storage y SQL Database. Las plataformas que no son soluciones de PaaS de Azure integradas requieran más esfuerzo. |
| Compatibilidad con la API | ¿: Es una API disponible que facilita la tarea para que funcione con la plataforma? Para Azure Table Service, hay un SDK con una API de .NET que admita el modelo de programación asincrónico de .NET 4.5. Si está escribiendo una aplicación. NET, será mucho más fácil escribir y probar el código para Azure Table Service en comparación con otra clave/valor columna almacén plataforma de datos que no tiene ninguna API o uno menos exhaustivo. |
| Coherencia de datos y la integridad transaccional | ¿-Es fundamental que la plataforma admite las transacciones con el fin de garantizar la coherencia de datos? Para realizar el seguimiento de correos electrónicos de masiva enviados, rendimiento y el costo de almacenamiento de datos baja podrían ser más importantes que la compatibilidad automática con las transacciones o la integridad referencial en la plataforma de datos, hacer que el servicio de tabla de Azure una buena elección. Para realizar el seguimiento de su cuenta bancaria equilibra o pedidos de compra de una plataforma de base de datos relacional que proporciona fuertes garantías transaccionales sería una opción mejor. |
| Continuidad del negocio | ¿-Fáciles son copias de seguridad, restauración y recuperación ante desastres? Tarde o temprano se dañan los datos de producción y tendrá una función de deshacer. Bases de datos relacionales a menudo tienen más capacidades de restauración específica, como la capacidad de restaurar a un momento dado. Comprender qué características de restauración están disponibles en cada plataforma que está considerando es un factor importante a tener en cuenta. |
| Costo | -Si más de una plataforma puede admitir la carga de trabajo de datos, ¿cómo se comparan en el costo? Por ejemplo, si usa ASP.NET Identity, puede almacenar datos de perfil de usuario en Azure Table Service o Azure SQL Database. Si no necesita la amplia consultar las funciones de base de datos SQL, puede elegir las tablas de Azure en parte porque cuesta mucho menos para una determinada cantidad de almacenamiento. |

¿Qué general, se recomienda es conocer la respuesta a las preguntas en cada una de estas categorías antes de elegir sus soluciones de almacenamiento de datos.

Además, la carga de trabajo puede tener requisitos específicos que algunas plataformas pueden admitir mejor que otros. Por ejemplo:

- ¿Necesita de su aplicación la capacidad de auditoría?
- ¿Cuáles son los requisitos de durabilidad de datos: ¿necesita capacidades de archivado o purga automatizadas?
- ¿Tienen necesidades de seguridad especializada? Por ejemplo, los datos incluyen PII (información de identificación personal), pero tiene que poder para asegurarse de que PII se excluye de los resultados de consulta.
- Si tiene algunos datos que no pueden almacenarse en la nube por razones legales o tecnológicas, puede necesitar una plataforma de almacenamiento de datos en la nube que facilita la integración con el almacenamiento local.

## <a name="demo--using-sql-database-in-azure"></a>Demostración: usar SQL Database en Azure

La aplicación Fix It utiliza una base de datos relacional para almacenar tareas. El script de Windows PowerShell de creación de entorno que se muestra en el [automatizar todo capítulo](automate-everything.md) crea dos instancias de base de datos SQL. Puede ver estas opciones en el portal, haga clic en el **bases de datos SQL** ficha.

![Bases de datos de SQL en el portal](data-storage-options/_static/image8.png)

También es fácil crear bases de datos mediante el portal.

Haga clic en **servicios de datos nuevos--** -- **base de datos SQL** -- **creación rápida**, escriba un nombre de base de datos, elija un servidor que ya tiene en su cuenta o cree uno nuevo y haga clic en **crear base de datos de SQL**.

![Crear una base de datos de SQL Database](data-storage-options/_static/image9.png)

Espere varios segundos, y tiene una base de datos en Azure listo para su uso.

![Crear nueva base de datos SQL](data-storage-options/_static/image10.png)

Por lo que Azure en unos pocos segundos, lo que puede que le lleve un día o una semana o más tiempo para llevar a cabo en el entorno local. Y puesto que podríamos puede crear bases de datos automáticamente en una secuencia de comandos o mediante una API de administración, pueden escalar dinámicamente al repartir los datos entre varias bases de datos < o:p >, siempre y cuando la aplicación se ha programado para que. </o : p >

Esto es un ejemplo de nuestro modelo de plataforma como servicio. No es necesario que administrar los servidores, lo hacemos. Si no tiene que preocuparse por las copias de seguridad, lo hacemos. Se está ejecutando en alta disponibilidad: los datos en la base de datos se replican automáticamente en tres servidores. Si se bloquea una máquina, se conmutarán por error automáticamente y no se pierde ningún dato. El servidor se aplican las revisiones con regularidad, no es necesario preocuparse de ello.

Haga clic en un botón y obtener la cadena de conexión exacta que necesita y puede comenzar a usar inmediatamente la nueva base de datos.

![Cadenas de conexión](data-storage-options/_static/image11.png)

El panel muestra el historial de conexión y la cantidad de almacenamiento utilizado.

![Panel SQL Database](data-storage-options/_static/image12.png)

Puede administrar las bases de datos en el portal o mediante el uso de herramientas de SQL Server ya está familiarizado con, incluidos SQL Server Management Studio (SSMS) y las herramientas de Visual Studio, el Explorador de objetos de SQL Server (SSOX) y el Explorador de servidores.

![SSOX](data-storage-options/_static/image13.png)

Otro aspecto interesante es el modelo de precios. Puede iniciar el desarrollo con una base de datos de 20 MB gratuita y se inicia una base de datos de producción aproximadamente 5 USD al mes. Pague solo para la cantidad de datos que realmente se almacena en la base de datos, no la capacidad máxima. No tienes que comprar una licencia.

Es fácil de escalar la base de datos de SQL. Para la aplicación Fix It, la base de datos que creamos en nuestra secuencia de comandos de automatización está limitado a 1 GB. Si desea ajustar la escala hasta 150 GB, puede ir al portal y cambiar esa configuración o ejecutar un comando de la API de REST y, en segundos tiene una base de datos de 150 GB que se pueden implementar datos en.

![Los tamaños y las ediciones de SQL Database](data-storage-options/_static/image14.png)

Que es la potencia de la nube para crear infraestructura de forma rápida y sencilla y empezar a usarlo inmediatamente.

La aplicación Fix It usa dos bases de datos SQL, uno para la suscripción (autenticación y autorización) y otro para los datos, y esto es todo lo que debe hacer para aprovisionarla y ampliarlo. Ya ha visto cómo aprovisionar las bases de datos a través de scripts de Windows PowerShell, y ahora también ha visto lo fácil que es hacer en el portal.

## <a name="entity-framework-versus-direct-database-access-using-adonet"></a>Entity Framework frente al acceso directo de la base de datos mediante ADO.NET

La aplicación Fix It tiene acceso a estas bases de datos mediante Entity Framework, Microsoft recomienda ORM (asignador relacional de objetos) para aplicaciones .NET. Un ORM es una excelente herramienta que facilita la productividad del desarrollador, pero la productividad es a costa de la disminución del rendimiento en algunos escenarios. En una aplicación de nube del mundo real no se puede tomar una decisión entre usar EF o usar ADO.NET directamente, usará ambos. La mayoría del tiempo cuando escribe código que funciona con la base de datos, obtener el máximo rendimiento no es importante y puede sacar partido de la codificación simplificada y las pruebas que obtendrá con Entity Framework. En situaciones donde la sobrecarga de EF provocaría un rendimiento aceptable, puede escribir y ejecutar sus propias consultas utilizando ADO.NET, idealmente mediante una llamada a procedimientos almacenados.

Cualquier método que use para tener acceso a la base de datos que desea minimizar "intercambio de mensajes" tanto como sea posible. En otras palabras, si puede obtener todos los datos que necesita en un resultado de consulta más grande establecido en lugar de decenas o cientos de otras más pequeñas, que es normalmente será preferible. Por ejemplo, si tiene que los estudiantes de la lista y los cursos que están inscrito en, es mejor obtener todos los datos de consulta de una combinación en lugar de introducción a los alumnos en una consulta y ejecutar consultas independientes para los cursos de cada alumno.

## <a name="sql-databases-and-the-entity-framework-in-the-fix-it-app"></a>Las bases de datos SQL y Entity Framework en la aplicación Fix It

En la aplicación Fix It el `FixItContext` (clase), que se deriva de Entity Framework `DbContext` clase, identifica la base de datos y especifica las tablas en la base de datos. El contexto especifica un conjunto de entidades (tabla) para las tareas y el código pasa al contexto del nombre de la cadena de conexión. Ese nombre hace referencia a una cadena de conexión que se define en el archivo Web.config.

[!code-csharp[Main](data-storage-options/samples/sample1.cs?highlight=4,8)]

La cadena de conexión en el *Web.config* archivo se denomina appdb (aquí que apunta a la base de datos de desarrollo local):

[!code-xml[Main](data-storage-options/samples/sample2.xml?highlight=3)]

Entity Framework crea un *FixItTasks* tabla según las propiedades incluidas en el `FixItTask` clase de entidad. Se trata de una clase simple de POCO (objeto CRL estándar), lo que significa no herede o tiene dependencias en Entity Framework. Pero Entity Framework sabe cómo crear una tabla basada en él y ejecutar operaciones de CRUD (crear-leer-actualizar-eliminar) con él.

[!code-csharp[Main](data-storage-options/samples/sample3.cs)]

![Tabla FixItTasks](data-storage-options/_static/image15.png)

La aplicación Fix It incluye una interfaz de repositorio que usa para trabajar con el almacén de datos de las operaciones CRUD.

[!code-csharp[Main](data-storage-options/samples/sample4.cs)]

Tenga en cuenta que los métodos de repositorio son todos async, por lo que todo acceso a datos puede realizarse de manera asincrónica por completo.

La implementación de repositorio llama a métodos asincrónicos de Entity Framework para trabajar con los datos, incluidas consultas LINQ, así como para insertar, actualizar y elimina operaciones. Este es un ejemplo del código para buscar una tarea Fix It.

[!code-csharp[Main](data-storage-options/samples/sample5.cs)]

Observará también hay algunos intervalos y código de registro de error aquí, veremos que más adelante en el [capítulo supervisión y telemetría](monitoring-and-telemetry.md).

<a id="sqliaas"></a>
## <a name="choosing-sql-database-paas-versus-sql-server-in-a-vm-iaas-in-azure"></a>Elegir base de datos SQL (PaaS) frente a SQL Server en una máquina virtual (IaaS) en Azure

Una ventaja de SQL Server y Azure SQL Database es que el modelo de programación principales para ambos es idéntico. Puede utilizar la mayoría de las mismas habilidades en ambos entornos. Incluso puede usar una base de datos de SQL Server en el desarrollo y una instancia de base de datos SQL en la nube, que es cómo se configura la aplicación Fix It.

Como alternativa, puede ejecutar el mismo servidor de SQL Server en la nube que ejecute de forma local mediante la instalación de las máquinas virtuales IaaS. Para algunas aplicaciones heredadas, que se ejecuta SQL Server en una máquina virtual podría ser una solución mejor. Dado que una base de datos de SQL Server se ejecuta en una máquina virtual dedicada, tiene más recursos a su disposición que una base de datos de la base de datos SQL que se ejecuta en un servidor compartido. Esto significa que una base de datos de SQL Server puede ser más grande y aún funcionan bien. En general, cuanto menor sea el tamaño de la base de datos y el tamaño de la tabla, mejor el caso de uso funciona para la base de datos de SQL (PaaS).

Estas son algunas directrices sobre cómo elegir entre los dos modelos.

| Base de datos SQL Azure (PaaS) | SQL Server en una máquina Virtual (IaaS) |
| --- | --- |
| **Los profesionales de** -no tendrá que crear o administrar máquinas virtuales, actualizar o aplicar revisiones de sistema operativo o SQL; Azure lo hace. -Alta disponibilidad integrada, con un SLA de nivel de base de datos. -Bajo costo total de propiedad (TCO) porque se paga solo por lo que usa (se necesita ninguna licencia). -Buenos para controlar un gran número de bases de datos menores (&lt;= 500 GB cada uno). -Fácil de crear dinámicamente nuevas bases de datos para habilitar el escalado horizontal. | ***Los profesionales de*** : característica compatible con un entorno local SQL Server. -Puede implementar SQL Server [alta disponibilidad mediante AlwaysOn](https://www.microsoft.com/sqlserver/solutions-technologies/mission-critical-operations/high-availability.aspx) en más de 2 máquinas virtuales, con el SLA de nivel de máquina virtual. -Tiene control completo sobre la administración de SQL. -Se pueden reutilizar las licencias SQL que ya posea o paga por hora para uno. -Buenos para controlar menos pero más grandes (1 TB +) bases de datos. |
| **Desventajas** -algunas características brechas en comparación con un entorno local de SQL Server (falta de [integración CLR](https://technet.microsoft.com/library/ms131102.aspx), [TDE](https://technet.microsoft.com/library/bb934049.aspx), [compatibilidad de compresión](https://technet.microsoft.com/library/cc280449.aspx), [SQL Server Reporting Services](https://technet.microsoft.com/library/ms159106.aspx), etc.): límite de tamaño de base de datos de 500 GB. | ***Desventajas*** : las actualizaciones o revisiones (sistema operativo y SQL) son responsabilidad suya: creación y administración de bases de datos son responsabilidad suya: disco IOPS (operaciones de entrada/salida por segundo), limitada a entre 8000 (a través de las unidades de datos de 16). |

Si desea usar SQL Server en una máquina virtual, puede usar su propia licencia de SQL Server, o puede pagar por una por hora. Por ejemplo, en el portal o mediante la API de REST puede crear una nueva máquina virtual con una imagen de SQL Server.

![Creación de máquina virtual con SQL Server](data-storage-options/_static/image16.png)

![Lista de imágenes de máquina virtual de SQL Server](data-storage-options/_static/image17.png)

Cuando se crea una máquina virtual con una imagen de SQL Server, se prorratea el costo de licencia de SQL Server por horas en función del uso de la máquina virtual. Si tiene un proyecto que solo se va a ejecutar un par de meses, es más barato que pagar por hora. Si piensa que el proyecto se va a último durante años, es más barato adquirir la licencia de la forma habitual.

## <a name="summary"></a>Resumen

Cloud computing hace que sea práctico mezclar y métodos de almacenamiento de datos de coincidencia mejor las necesidades de su aplicación. Si está creando una nueva aplicación, medite concienzudamente sobre las preguntas que se muestran aquí para elegir los enfoques que seguirán funcionando bien cuando vaya creciendo su aplicación. El [siguiente capítulo](data-partitioning-strategies.md) explica algunas estrategias de partición que puede usar para combinar varios enfoques de almacenamiento de datos.

## <a name="resources"></a>Recursos

Para obtener más información, vea los siguientes recursos.

Seleccionar una plataforma de base de datos:

- [Acceso a datos para soluciones altamente escalables: uso de SQL, NoSQL y persistencia políglota](http://aka.ms/dag-doc). Libro electrónico de Microsoft Patterns and Practices que entra en profundidad en los diferentes tipos de datos almacena disponibles para las aplicaciones en la nube.
- [Microsoft Patterns and Practices - Guía de Azure](https://msdn.microsoft.com/library/ff898430.aspx). Consulte el manual de coherencia de datos, replicación de datos y orientación de sincronización, patrón Index Table, el patrón Materialized View.
- [BASE: Alternativa Acid](http://queue.acm.org/detail.cfm?id=1394128). El artículo sobre un equilibrio entre la coherencia de los datos y la escalabilidad.
- [Siete bases de datos de siete semanas: una guía para las bases de datos modernas y el movimiento NoSQL](https://www.amazon.com/Seven-Databases-Weeks-Modern-Movement/dp/1934356921). Libro de Eric Redmond y Jim R. Wilson. Se recomienda encarecidamente para introducir usted mismo a la gama de plataformas de almacenamiento de datos disponibles en la actualidad.

Elección entre SQL Server y base de datos SQL:

- [Vista previa Premium de orientación de la base de datos SQL](https://msdn.microsoft.com/library/windowsazure/dn369873.aspx). Introducción a SQL Database Premium y orientación sobre cuándo elegir él a través de las ediciones de SQL Database Web Edition y Business.
- [Instrucciones y limitaciones (Azure SQL Database)](https://msdn.microsoft.com/library/windowsazure/ff394102.aspx). Página del portal que contiene vínculos a documentación sobre las limitaciones de SQL Database, incluida una que se centra en características de SQL Server de esa base de datos de SQL no es compatible con.
- [SQL Server en máquinas virtuales de Azure](https://msdn.microsoft.com/library/windowsazure/jj823132.aspx). Página del portal que contiene vínculos a documentación sobre la ejecución de SQL Server en Azure.
- [Scott Guthrie se explican las bases de datos SQL de Azure](https://azure.microsoft.com/documentation/videos/sql-in-azure-scottgu/). 6 minutos vídeo de introducción a la base de datos de SQL por Scott Guthrie.
- [Patrones de aplicaciones y estrategias de desarrollo para SQL Server en Azure Virtual Machines](https://msdn.microsoft.com/library/windowsazure/dn574746.aspx).

Uso de Entity Framework y la base de datos SQL en una aplicación Web de ASP.NET

- [Introducción a EF 6 mediante MVC 5](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Serie de tutoriales de nueve partes que le guiará a través de la creación de una aplicación MVC que usa EF y la base de datos implementa en Azure y SQL Database.
- [Implementación Web de ASP.NET con Visual Studio](../../../../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md). Doce serie de tutoriales que entra en más profundidad sobre cómo implementar una base de datos mediante el uso de EF Code First.
- [Implementar una aplicación ASP.NET MVC 5 segura con pertenencia, OAuth y SQL Database en un sitio Web de Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/). Tutorial paso a paso que explica cómo crear una aplicación web que usa la autenticación, almacena las tablas de la aplicación en la base de datos de pertenencia, modifica el esquema de base de datos y la aplicación implementa en Azure.
- [Mapa de contenido de acceso a datos ASP.NET](https://go.microsoft.com/fwlink/p/?LinkId=282414). Vínculos a recursos para trabajar con EF y base de datos SQL.

Uso de MongoDB en Azure:

- [MongoLab - MongoDB en Azure](http://msopentech.com/opentech-projects/mongolab-mongodb-on-windows-azure/). Página del portal para obtener documentación sobre la ejecución de MongoDB en Azure.
- [Crear un sitio web de Azure que se conecta a MongoDB en una máquina virtual en Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-store-data-mongodb-vm/). Tutorial paso a paso que se muestra cómo usar una base de datos de MongoDB en una aplicación web ASP.NET.

HDInsight (Hadoop en Azure):

- [HDInsight](https://azure.microsoft.com/documentation/services/hdinsight/). Documentación de HDInsight en el portal la [Azure](https://azure.microsoft.com/) sitio Web.
- [Hadoop y HDInsight: Big Data en Azure](https://msdn.microsoft.com/magazine/dn385705.aspx). Artículo de MSDN Magazine, Bruno Terkaly y Ricardo Villalobos, introducción a Hadoop en Azure.
- [Microsoft Patterns and Practices - Guía de Azure](https://msdn.microsoft.com/library/dn568099.aspx). Consulte modelo de MapReduce.

> [!div class="step-by-step"]
> [Anterior](single-sign-on.md)
> [Siguiente](data-partitioning-strategies.md)
