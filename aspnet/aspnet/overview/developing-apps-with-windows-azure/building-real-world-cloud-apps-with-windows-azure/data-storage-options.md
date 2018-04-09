---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options
title: Opciones de almacenamiento de datos (creación de aplicaciones de nube reales con Azure) | Documentos de Microsoft
author: MikeWasson
description: Las aplicaciones de nube de creación Real World con libros electrónicos Azure se basa en una presentación desarrollada por Scott Guthrie. Se explican 13 patrones y prácticas recomendadas que puede...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: e51fcecb-cb33-4f9e-8428-6d2b3d0fe1bf
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options
msc.type: authoredcontent
ms.openlocfilehash: d638dca331cb24c340a4471e5964a00b75bb608a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="data-storage-options-building-real-world-cloud-apps-with-azure"></a>Opciones de almacenamiento de datos (creación de aplicaciones de nube reales con Azure)
====================
por [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Descarga corregirlo proyecto](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) o [descargar libros electrónicos](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> El **compilar aplicaciones de nube de mundo Real con Azure** libros electrónicos se basa en una presentación desarrollada por Scott Guthrie. Se explican los 13 patrones y prácticas recomendadas que le permitirá tener éxito el desarrollo de aplicaciones web para la nube. Para obtener información sobre el libro electrónico, consulte [el primer capítulo](introduction.md).


Mayoría de los usuarios se usa para bases de datos relacionales, y que tienden a pasar por alto otras opciones de almacenamiento de datos cuando diseña una aplicación de nube. El resultado puede ser poco óptimo rendimiento, los gastos de alta o lo que es peor, porque [NoSQL](http://en.wikipedia.org/wiki/NoSQL) bases de datos (no relacionales) pueden administrar algunas tareas de forma más eficiente que las bases de datos relacionales. Cuando los clientes nos solicita ayuda para resolver un problema de almacenamiento de datos críticos, suele ser porque tienen una base de datos relacional en una de las opciones de NoSQL habría funcionado mejor. En aquellos casos en el cliente habría sido mejor que si hubiera implementado la solución NoSQL antes de implementar la aplicación en producción.

Por otro lado, también sería un error que supongan que una base de datos NoSQL puede hacer todo lo que tienen o son lo suficientemente bien. No hay ninguna elección de administración de datos recomendada único para todas las tareas de almacenamiento de datos; soluciones de administración de datos diferentes están optimizadas para diferentes tareas. Mayoría de las aplicaciones reales en la nube tiene una gran variedad de requisitos de almacenamiento de datos y a menudo se atienden mejor mediante una combinación de varias soluciones de almacenamiento de datos.

El propósito de este capítulo es proporcionarle un sentido más amplio de las opciones de almacenamiento de datos disponibles para una aplicación de nube y algunas instrucciones básicas sobre cómo elegir las que se ajusten a su escenario. Es mejor tener en cuenta las opciones disponibles para usted y pensar en sus puntos fuertes y débiles antes de desarrollar una aplicación. Cambiar las opciones de almacenamiento de datos en una aplicación de producción puede ser muy difícil, como tener que cambiar un motor jet mientras el plano está en tránsito.

## <a name="data-storage-options-on-azure"></a>Opciones de almacenamiento de datos de Azure

La nube facilita relativamente fáciles de usar una variedad de relacional y almacenes de datos NoSQL. Estas son algunas de las plataformas de almacenamiento de datos que puede usar en Azure.

![](data-storage-options/_static/image1.png)

La tabla muestran cuatro tipos de bases de datos SQL:

- [Las bases de datos de clave/valor](https://msdn.microsoft.com/library/dn313285.aspx#sec7) almacenar un objeto serializado único para cada valor de clave. Son buenas para almacenar grandes volúmenes de datos donde desea obtener un elemento de un determinado valor de clave y no tiene para consultar en función de otras propiedades del elemento.

    [Almacenamiento de blobs de Azure](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/) es una base de datos de clave/valor que funciona como el almacenamiento de archivos en la nube, con valores de clave que se corresponden con los nombres de archivo y carpeta. Recuperar un archivo de su carpeta y el nombre de archivo, no mediante la búsqueda de valores en el contenido del archivo.

    [El almacenamiento de tabla](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/) también es una base de datos de clave/valor. Cada valor se denomina un *entidad* (similar a una fila, identificada por una clave de partición y la clave de fila) y contiene varias *propiedades* (similar a las columnas, pero no todas las entidades de una tabla deben compartir el mismo columnas). Realizar consultas en columnas distintas de la clave es muy eficaz y deben evitarse. Por ejemplo, puede almacenar datos de perfil de usuario, con una partición que almacena información sobre un único usuario. Puede almacenar datos, como el nombre de usuario, hash de contraseña, la fecha de nacimiento y así sucesivamente, en propiedades independientes de una entidad o entidades independientes en la misma partición. Pero, no deseará consultar todos los usuarios con un determinado intervalo de fechas de nacimiento y no se puede ejecutar una consulta de combinación entre la tabla de perfiles y otra tabla. Almacenamiento de tabla es más escalable y menos costoso que una base de datos relacional, pero no habilita las consultas complejas o combinaciones.
- [Documentdatabases](https://msdn.microsoft.com/library/dn313285.aspx#sec8) son bases de datos de clave/valor en el que los valores son *documentos*. "Documento" no se usa en el sentido de un documento de Word o Excel, pero significa que una colección de campos con nombre y valores, que podría ser un documento secundario. Por ejemplo, en una tabla de historial de pedidos podría tener un documento de pedido número de pedido, fecha de pedido y campos del cliente; y el campo de cliente puede tener campos de nombre y dirección. La base de datos codifica los datos de campo en un formato como XML, YAML, JSON o BSON; o bien, puede usar texto sin formato. Una característica que establece las bases de datos de documentos además de las bases de datos de clave/valor es la capacidad de consultar en los campos no son de clave y definir índices secundarios para simplificar las consultas más eficaz. Esta capacidad hace que una base de datos de documentos más adecuada para aplicaciones que necesitan recuperar datos en función de criterios más complejos que el valor de la clave de documento. Por ejemplo, en una base de datos del documento de historial de pedido de ventas podría consultar en diversos campos como Id. de producto, Id. de cliente, el nombre del cliente y así sucesivamente. [MongoDB](http://www.mongodb.org/) es una base de datos de documentos populares.
- [Las bases de datos de columna familia](https://msdn.microsoft.com/library/dn313285.aspx#sec9) son almacenes de datos que le permiten al almacenamiento de datos de estructura en colecciones de columnas relacionadas denominadas familias de columna de clave/valor. Por ejemplo, una base de datos del censo podría tener un grupo de columnas para el nombre de una persona (primero, segundo nombre, apellidos), un grupo de direcciones de la persona y un grupo de información de perfil de la persona (fecha de nacimiento, sexo, etcetera.). La base de datos, a continuación, puede almacenar cada familia de columna en una partición independiente manteniendo todos los datos de una persona relacionada con la misma clave. A continuación, puede leer toda la información de perfil sin tener que leer toda la información de nombre y la dirección. [Casandra](http://cassandra.apache.org/) es una base de datos de columna familia popular.
- [Las bases de datos del gráfico](https://msdn.microsoft.com/library/dn313285.aspx#sec10) almacenar información como una colección de objetos y relaciones. El propósito de una base de datos de gráfico es permitir que una aplicación realizar eficazmente las consultas que atraviesan la red de objetos y las relaciones entre ellos. Por ejemplo, los objetos podrían ser los empleados en una base de datos de recursos humanos y es recomendable para facilitar las consultas como "buscan todos los empleados que trabajan de forma directa o indirectamente de Scott". [Neo4j](http://www.neo4j.org/) es una base de datos de gráfico populares.

En comparación con las bases de datos relacionales, las opciones de NoSQL ofrecen mayor escalabilidad y la rentabilidad para el almacenamiento y análisis de datos no estructurados. La contrapartida es que no proporcionan la queryability enriquecido y funcionalidades de integridad de datos eficaces de bases de datos relacionales. NoSQL podría funcionar bien para los datos de registro IIS, lo que implica gran volumen sin necesidad de consultas de combinación. NoSQL no funciona tan bien para las bancarias en las transacciones, que requiere la integridad de los datos absoluta e implica varias relaciones con otros datos relacionados con la cuenta.

También hay una categoría más reciente de la plataforma de base de datos denominada [NewSQL](http://en.wikipedia.org/wiki/NewSQL) que combina la escalabilidad de una base de datos NoSQL con la queryability y la integridad transaccional de una base de datos relacional. Las bases de datos de NewSQL están diseñados para distribuida almacenamiento y procesamiento de consultas, que a menudo es difícil de implementar en las bases de datos de "OldSQL". [NuoDB](http://www.nuodb.com/) es un ejemplo de una base de datos de NewSQL que puede usarse en Azure.

<a id="hadoop"></a>
## <a name="hadoop-and-mapreduce"></a>Hadoop y MapReduce

Los grandes volúmenes de datos que se pueden almacenar en las bases de datos NoSQL pueden resultar difíciles analizar eficazmente de forma puntual. Para hacer que puede usar un marco como [Hadoop](http://hadoop.apache.org/) que implementa [MapReduce](http://en.wikipedia.org/wiki/MapReduce) funcionalidad. Básicamente hace un proceso de MapReduce de qué es el siguiente:

- Limitar el tamaño de los datos que deben procesarse mediante la selección de los datos del almacén solo los datos que realmente necesita analizar. Por ejemplo, desea conocer la composición de la base por año de nacimiento del usuario para seleccionar sólo los años de nacimiento fuera de su almacén de datos de perfil de usuario.
- Desglosar los datos en partes y enviarlos a otros equipos para su procesamiento. El equipo A calcula el número de personas con las fechas de 1950 1959, el equipo B hace 1960-1969, etcetera. Este grupo de equipos se denomina un *clúster de Hadoop*.
- Reunir los resultados de cada parte vuelve una vez terminado el procesamiento en las partes. Ahora tiene una lista relativamente corta de cuántas personas para cada año de nacimiento y la tarea de calcular los porcentajes en esta lista global es fácil de administrar.

En Azure, [HDInsight](https://azure.microsoft.com/services/hdinsight/) permite procesar, analizar y obtener una nueva percepción de big data con la potencia de Hadoop. Por ejemplo, podría utilizar para analizar los registros de servidor web:

- Habilitar el registro de servidor web para la cuenta de almacenamiento. Esto configura Azure para escribir registros en el servicio Blob para todas las solicitudes HTTP a la aplicación. El servicio Blob es básicamente el almacenamiento de archivos en la nube, y se integra perfectamente con HDInsight. 

    ![Registros de almacenamiento de blobs](data-storage-options/_static/image2.png)
- Como la aplicación aumenta el tráfico, se escriben los registros IIS del servidor web al almacenamiento de blobs. 

    ![Registros de servidor Web](data-storage-options/_static/image3.png)
- En el portal, haga clic en **New** - **Data Services** - **HDInsight** - **creación rápida**, y especifique un nombre de clúster de HDInsight, el tamaño de clúster (número de nodos de datos del clúster de HDInsight) y un nombre de usuario y una contraseña para el clúster de HDInsight. 

    ![HDInsight](data-storage-options/_static/image4.png)

Ahora puede configurar trabajos de MapReduce para analizar los registros y obtenga respuestas a preguntas como:

- ¿Qué horas del día mi aplicación obtener el tráfico o más?
- ¿Qué países es mi el tráfico procedente de?
- ¿Cuál es el ingreso de entorno promedio de las áreas de de que mi tráfico procede. (Hay un conjunto de datos público que proporciona los ingresos de entorno mediante una dirección IP, y puede hacer coincidir con la dirección IP en los registros del servidor web).
- ¿Cómo se correlacionan los ingresos de entorno a páginas específicas o productos en el sitio?

A continuación, podría utilizar las respuestas a preguntas como estas para dirigir los anuncios según la probabilidad de en que un cliente podría estar interesado o probables que compren un producto determinado.

Como se explica en la [automatizar todo capítulo](automate-everything.md), se puede automatizar la mayoría de las funciones que puede realizar en el portal y que incluye la configuración y ejecución de los trabajos de análisis de HDInsight. Una secuencia de comandos de HDInsight típico podría contener los siguientes pasos:

- Aprovisiona un clúster de HDInsight y vincularlo a la cuenta de almacenamiento para la entrada de almacenamiento de blobs.
- Cargue los archivos ejecutables de trabajo de MapReduce (archivos .jar o .exe) en el clúster de HDInsight.
- Enviar un MapReduce que almacena los datos de salida en almacenamiento de blobs.
- Espere a que el trabajo finalice.
- Eliminar el clúster de HDInsight.
- Acceso a la salida del almacenamiento de blobs.

Al ejecutar un script que hace todo esto, se minimiza la cantidad de tiempo que se aprovisione el clúster de HDInsight, lo que reduce los costos.

<a id="paasiaas"></a>
## <a name="platform-as-a-service-paas-versus-infrastructure-as-a-service-iaas"></a>Plataforma como servicio (PaaS) frente a la infraestructura como servicio (IaaS)

Las opciones de almacenamiento de datos enumeradas anteriormente incluyen plataforma como-servicio (PaaS) y las soluciones de infraestructura como-servicio (IaaS). PaaS significa que se administra la infraestructura de hardware y software y que usar el servicio. Base de datos SQL es una característica de PaaS de Azure. Preguntar para bases de datos, y en segundo plano Azure configura y configura las máquinas virtuales y configura las bases de datos en ellas. No tiene acceso directo a las máquinas virtuales y no es necesario administrarlos. IaaS significa que instalar, configura y administrar máquinas virtuales que se ejecutan en nuestra infraestructura de centro de datos y colocar todo lo que desees en ellos. Proporcionamos una galería de imágenes de máquina virtual configuradas previamente para configuraciones de máquina virtual común. Por ejemplo, puede instalar las imágenes de máquina virtual configuradas previamente para Windows Server 2008, Windows Server 2012, BizTalk Server, Oracle WebLogic Server, base de datos de Oracle, etcetera.

Soluciones de PaaS de datos que ofrece Azure incluyen:

- Azure base de datos de SQL (anteriormente conocida como SQL Azure). En la nube base de datos relacional basada en SQL Server.
- Almacenamiento de tabla. Un clave y valor base de datos NoSQL.
- Almacenamiento de blobs de Azure. Almacenamiento de archivos en la nube.

IaaS, puede ejecutar cualquier cosa que puede cargar en una máquina virtual, por ejemplo:

- Bases de datos relacionales, como SQL Server, Oracle, MySQL, SQL Compact, SQLite o Postgres.
- Almacenes de datos de clave/valor como Memcached, Redis, Casandra y Riak.
- Almacenes de datos de columna como HBase.
- Bases de datos de documento como MongoDB, RavenDB y CouchDB.
- Bases de datos de gráfico como Neo4j.

![Opciones de almacenamiento de datos de Azure](data-storage-options/_static/image5.png)

La opción de IaaS ofrece opciones de almacenamiento de datos prácticamente ilimitada y muchas de ellas son especialmente fácil de usar porque se pueden crear máquinas virtuales con imágenes preconfiguradas. Por ejemplo, en el portal de administración vaya a **máquinas virtuales**, haga clic en el **imágenes** ficha y haga clic en **examinar VM Depot**.

![Examinar VM Depot](data-storage-options/_static/image6.png)

A continuación, verá una lista de [centenares de imágenes de máquinas virtuales preconfiguradas](http://www.hanselman.com/blog/Over400VirtualMachineImagesOfOpenSourceSoftwareStacksInTheVMDepotAzureGallery.aspx), y puede crear una máquina virtual desde una imagen que tiene preinstalado un sistema de administración de base de datos, por ejemplo, MongoDB, Neo4J, Redis, Casandra o CouchDB:

![MongoDB en VM Depot](data-storage-options/_static/image7.png)

Azure hace tan fáciles de usar como posibles opciones de almacenamiento de datos de IaaS, pero las ofertas de PaaS tienen numerosas ventajas que las hacen más rentable y práctico para muchos escenarios:

- No tienes que crear las máquinas virtuales, que usar el portal o una secuencia de comandos para configurar un almacén de datos. Si desea que un almacén de datos de 200 terabytes, puede simplemente haga clic en un botón o ejecutar un comando y, en segundos está listo para su uso.
- No tiene que administrar o aplicar parches a las máquinas virtuales que se usa el servicio; Microsoft lo hace automáticamente.-no tiene que preocuparse sobre cómo configurar la infraestructura para la escala o de alta disponibilidad; Microsoft administra todo esto por usted.
- No es necesario comprar licencias; las tarifas de licencia se incluyen en las cuotas de servicio.
- Solo paga por lo que usa.

Opciones de almacenamiento de datos de PaaS en Azure incluyen ofertas de proveedores de terceros. Por ejemplo, puede elegir la [complemento MongoLab](https://azure.microsoft.com/documentation/articles/store-mongolab-web-sites-dotnet-store-data-mongodb/) desde la tienda de Azure para proporcionar una base de datos de MongoDB como un servicio.

## <a name="choosing-a-data-storage-option"></a>Elegir una opción de almacenamiento de datos

No hay un enfoque es adecuado para todos los escenarios. Si alguien indica que esta tecnología es la respuesta, lo primero que ponerse en contacto es "¿Cuál es la pregunta?", porque diferentes soluciones están optimizadas para cosas diferentes. Hay ventajas definidas en el modelo relacional; por este motivo ha estado presente durante mucho. Pero también son lados de abajo a SQL que pueden tratarse con una solución de NoSQL.

A menudo lo que se ve mejor trabajo es un enfoque composicional, donde se utilicen SQL y NoSQL en una única solución. Incluso cuando dicen que va a adoptar NoSQL, si obtiene los detalles sobre lo que está haciendo a menudo buscar que están usando varios marcos de trabajo de NoSQL diferentes: está usando [CouchDB](http://wiki.apache.org/couchdb/Introduction), y [Redis](http://redis.io/)y [ Riak](http://basho.com/riak/) para cosas diferentes. Incluso Facebook, que usa ampliamente NoSQL, usa una diversidad de marcos de NoSQL para las distintas partes del servicio. La flexibilidad de mezclar y combinar los métodos de almacenamiento de datos es una de las cosas que "nice" acerca de la nube, porque es fácil de usar varias soluciones de datos e integrarlos en una sola aplicación.

Estas son algunas preguntas pensar al elegir un enfoque:

| Semántica de datos | -¿Qué es el almacenamiento y datos acceso a datos principales semántico (¿está almacenando datos relacionales o no estructurados)? Datos no estructurados como archivos multimedia apropiada en el almacenamiento de blobs; una colección de datos relacionados, como los productos, inventarios, proveedores, pedidos de clientes, etc., se adapta mejor para una base de datos relacional. |
| --- | --- |
| La compatibilidad con consultas | -¿Resulta sencillo para consultar los datos? -¿Qué tipos de preguntas pueden ser eficaz más frecuentes? Almacenes de datos de clave/valor son muy buenas para obtener una sola fila tiene un valor de clave pero no tan buena para consultas complejas. Para un almacén de datos de perfil de usuario donde siempre obtiene los datos para un usuario determinado, un almacén de datos de clave/valor podría funcionar bien; para un catálogo de productos donde desea obtener agrupaciones diferentes en función de varios atributos de producto podría funciona mejor en una base de datos relacional. Bases de datos NoSQL pueden almacenar grandes volúmenes de datos de forma eficaz, pero tendrá que la estructura de la base de datos alrededor de la forma en que la aplicación consulta los datos y esto complica las consultas ad hoc realizar. Con una base de datos relacional, puede crear casi cualquier tipo de consulta. |
| Proyección funcional | -Puede preguntas, agregaciones, etc., ser ejecutado en el servidor? Si ejecutar SELECT COUNT (\*) de una tabla de SQL, muy eficazmente se hacen todo el trabajo en el servidor y devuelve el número que me interesa. Si desea el mismo cálculo de un almacén de datos NoSQL que no admite la agregación, esto es una "unbounded consulta" ineficaz y probablemente superará el tiempo de espera. Incluso si la consulta se realiza correctamente tengo que recuperar todos los datos del servidor al cliente y contar las filas en el cliente. -¿Qué idiomas o tipos de expresiones se pueden usar? ¿Puedo usar SQL con una base de datos relacional. Con algunas bases de datos NoSQL, como el almacenamiento de tabla de Azure, vamos a usar [OData](http://www.odata.org/), y todo lo que pueda hacer es filtrar en la clave principal y obtener proyecciones (seleccionar un subconjunto de los campos disponibles). |
| Facilidad de escalabilidad | ¿-La frecuencia y el cantidad se necesitan los datos ajustar la escala? ¿-Implementa la plataforma de forma nativa escalado horizontal? -¿Resulta sencillo para agregar o quitar capacidad (rendimiento y tamaño)? Tablas y bases de datos relacionales no están automáticamente tienen particiones para que sean escalables, por lo que son difíciles de escalar más allá de ciertas limitaciones. Almacenes de datos NoSQL como el almacenamiento de Azure Table inherentemente dividir todo, y no hay casi ningún límite en cuanto a agregar particiones. Puede escalar fácilmente el almacenamiento de tabla hasta 200 terabytes, pero el tamaño máximo de la base de datos de base de datos de SQL Azure es de 500 gigabytes. Puede escalar los datos relacionales mediante la partición en varias bases de datos, pero la configuración de una aplicación para admitir ese modelo implica mucho trabajo de programación. |
| Instrumentación y facilidad de uso | ¿-Sencilla es la plataforma para instrumentar, supervisar y administrar? Debe mantenerse informado sobre el estado y el rendimiento de su almacén de datos, por lo que necesita saber por adelantado qué métricas una plataforma ofrece de forma gratuita y lo que tiene que desarrollar por sí mismo. |
| Operaciones | ¿-Sencilla es la plataforma para implementar y ejecutar en Azure? ¿PaaS? ¿IaaS? ¿Linux? Almacenamiento de tabla y base de datos SQL son fáciles de configurar en Azure. Plataformas que no son soluciones integradas de PaaS de Azure requieran más esfuerzo. |
| Compatibilidad con la API | ¿: Es una API disponible que hace más fácil trabajar con la plataforma? No hay un SDK con una API de .NET que admita el modelo de programación asincrónico de .NET 4.5 para el servicio de la tabla de Azure. Si va a escribir una aplicación. NET, sea mucho más fácil escribir y probar el código para el servicio de la tabla de Azure en comparación con otra clave/valor columna almacén plataforma de datos que no tiene ninguna API o uno menos exhaustiva. |
| Coherencia de datos y la integridad transaccional | ¿-Es fundamental que la plataforma admite las transacciones con el fin de garantizar la coherencia de datos? Para realizar el seguimiento de los correos electrónicos de forma masiva enviados, rendimiento y el costo de almacenamiento de datos baja podrían ser más importantes que la compatibilidad automática con para las transacciones o la integridad referencial en la plataforma de datos, hacer que el servicio de la tabla de Azure una buena opción. Para realizar el seguimiento de cuenta bancaria equilibra o pedidos de compra de una plataforma de base de datos relacional que proporciona fuertes garantías transaccionales sería una opción mejor. |
| Continuidad del negocio | ¿-Lo fáciles son copias de seguridad, restauración y recuperación ante desastres? Antes o después se dañan los datos de producción y tendrá una función de deshacer. Bases de datos relacionales a menudo tienen más capacidades de restauración específica, como la capacidad de restaurar a un momento dado. Descripción de las características de restauración están disponibles en cada plataforma que está pensando es un factor importante a tener en cuenta. |
| Costo | -Si más de una plataforma puede admitir la carga de trabajo de datos, ¿cómo se comparan en el costo? Por ejemplo, si utiliza ASP.NET Identity, puede almacenar datos de perfil de usuario en el servicio de la tabla de Azure o base de datos de SQL Azure. Si no necesita la amplia consulta de funciones de base de datos SQL, puede elegir tablas de Azure en parte porque cuesta mucho menos, para una determinada cantidad de almacenamiento. |

¿Qué generalmente recomendamos es conocer la respuesta a las preguntas en cada una de estas categorías antes de elegir las soluciones de almacenamiento de datos.

Además, la carga de trabajo puede tener requisitos específicos que algunas plataformas pueden admitir mejor que otros. Por ejemplo:

- ¿Necesita de la aplicación la capacidad de auditoría?
- ¿Cuáles son los requisitos de durabilidad de datos: ¿necesita capacidades de archivado o purga automatizadas?
- ¿Tiene necesidades de seguridad especializada? Por ejemplo, los datos incluyen PII (información de identificación personal), pero se deben poder para asegurarse de que PII está excluido de los resultados de la consulta.
- Si tiene algunos datos que no se puede almacenar en la nube por motivos normativos o tecnológicas, tendrá que una plataforma de almacenamiento de datos en la nube que facilita la integración con el almacenamiento local.

## <a name="demo--using-sql-database-in-azure"></a>Demostración: uso de base de datos SQL de Azure

La aplicación repararlo utiliza una base de datos relacional para almacenar tareas. El script de Windows PowerShell de creación de entorno se muestra en el [automatizar todo capítulo](automate-everything.md) crea dos instancias de base de datos SQL. Puede ver estas opciones en el portal, haga clic en el **bases de datos SQL** ficha.

![Bases de datos de SQL en el portal](data-storage-options/_static/image8.png)

También es fácil crear las bases de datos a través del portal.

Haga clic en **nuevos--Data Services** -- **base de datos SQL** -- **creación rápida**, escriba un nombre de base de datos, elija un servidor que ya tiene en su cuenta o cree uno nuevo y haga clic en **crear base de datos de SQL**.

![Crear una base de datos de SQL Database](data-storage-options/_static/image9.png)

Espere unos segundos, y tiene una base de datos en Azure listo para su uso.

![Crear nueva base de datos SQL](data-storage-options/_static/image10.png)

Por lo que Azure dentro de unos segundos lo que puede que le lleve un día o a la semana o más tiempo para llevar a cabo en el entorno local. Y puesto igual de sencillo que puede crear las bases de datos automáticamente en una secuencia de comandos o mediante una API de administración, puede dinámicamente escalar horizontalmente por repartir los datos entre varias bases de datos de < o: p >, siempre y cuando la aplicación se ha programado para esa. </o : p >

Este es un ejemplo de nuestro modelo de plataforma como servicio. No es necesario que administrar los servidores, lo hacemos. No tiene que preocuparse de las copias de seguridad, lo hacemos. Se está ejecutando en la alta disponibilidad: los datos en la base de datos se replican automáticamente entre los tres servidores. Si una máquina muere, se conmutarán por error automáticamente y no se perderá ningún dato. El servidor se han modificado con regularidad, no es necesario preocuparse de ello.

Haga clic en un botón y obtiene la cadena de conexión exacta que necesita y puede empezar a usar la nueva base de datos.

![Cadenas de conexión](data-storage-options/_static/image11.png)

El panel muestra el historial de conexión y la cantidad de almacenamiento utilizado.

![Panel de la base de datos SQL](data-storage-options/_static/image12.png)

Puede administrar las bases de datos en el portal o mediante el uso de herramientas de SQL Server ya está familiarizado con, incluidos SQL Server Management Studio (SSMS) y las herramientas de Visual Studio, el Explorador de objetos de SQL Server (SSOX) y el Explorador de servidores.

![SSOX](data-storage-options/_static/image13.png)

Otro aspecto interesante es el modelo de precios. Puede iniciar el desarrollo con una base de datos gratuita de 20 MB, y una base de datos de producción se inicia en unos 5 dólares al mes. Se paga solo para la cantidad de datos que realmente se almacena en la base de datos, no la capacidad máxima. No tienes que adquirir una licencia.

Base de datos SQL es fácil de escalar. Para la aplicación repararlo, creamos en nuestro script de automatización de la base de datos está limitado a 1 GB. Si desea escalar hasta 150 GB, puede ir al portal y modificar la configuración o ejecutar un comando de API de REST y en segundos tiene una base de datos de 150 GB que pueda implementar datos en.

![Los tamaños y las ediciones de base de datos SQL](data-storage-options/_static/image14.png)

Que representa la potencia de la nube para crear infraestructura de forma rápida y sencilla y empezar a usar inmediatamente.

La aplicación repararlo usa dos bases de datos SQL, uno para la suscripción (autenticación y autorización) y otro para los datos, y esto es todo lo que tiene que hacer para aprovisionarla y ampliarlo. Se ha visto anteriormente cómo aprovisionar las bases de datos a través de scripts de Windows PowerShell, y ahora también ha visto lo fácil que es hacer en el portal.

## <a name="entity-framework-versus-direct-database-access-using-adonet"></a>Entity Framework frente al acceso directo de la base de datos mediante ADO.NET

La aplicación repararlo tiene acceso a estas bases de datos mediante el uso de Entity Framework, Microsoft recomienda ORM (asignador relacional de objetos) para las aplicaciones .NET. Un ORM es una herramienta excelente que facilita la productividad del desarrollador, pero productividad es a costa de la reducción del rendimiento en algunos escenarios. En una aplicación del mundo real en la nube no va a efectuar una elección entre usar EF o directamente con ADO.NET, deberá usar ambos. Mayoría de las veces cuando va a escribir código que funcione con la base de datos, obtener un rendimiento máximo no es crítico y puede aprovechar las ventajas de la codificación simplificada y de prueba que obtendrá con Entity Framework. En situaciones donde la sobrecarga de EF causaría un rendimiento inaceptable, puede escribir y ejecutar sus propias consultas utilizando ADO.NET, idealmente mediante una llamada a procedimientos almacenados.

Cualquier método que se utiliza para tener acceso a la base de datos que desea minimizar el "intercambio de mensajes" tanto como sea posible. En otras palabras, si puede obtener todos los datos que necesita en un resultado de consulta más amplio establecido en lugar de docenas o cientos de pequeñas, que es normalmente será preferible. Por ejemplo, si necesita estudiantes de lista y los cursos están inscritos en, es mejor obtener todos los datos de consulta de una combinación en lugar de obtener los alumnos en una consulta y ejecutar consultas independientes para cursos de cada alumno.

## <a name="sql-databases-and-the-entity-framework-in-the-fix-it-app"></a>Bases de datos SQL y Entity Framework en la aplicación repararlo

En la aplicación repararlo el `FixItContext` (clase), que se deriva de Entity Framework `DbContext` clase, identifica la base de datos y especifica las tablas en la base de datos. El contexto especifica un conjunto de entidades (tabla) para las tareas y el código pasa al contexto del nombre de la cadena de conexión. Ese nombre hace referencia a una cadena de conexión que se define en el archivo Web.config.

[!code-csharp[Main](data-storage-options/samples/sample1.cs?highlight=4,8)]

La cadena de conexión en el *Web.config* archivo se denomina appdb (aquí que apunta a la base de datos de desarrollo local):

[!code-xml[Main](data-storage-options/samples/sample2.xml?highlight=3)]

Entity Framework crea un *FixItTasks* tabla según las propiedades incluidas en la `FixItTask` clase de entidad. Se trata de una clase simple de POCO (objeto CLR antiguos sin formato), lo que significa no heredan de o tienen alguna dependencia en Entity Framework. Pero Entity Framework sabe cómo crear una tabla basada en ella y ejecutar las operaciones CRUD (crear-lectura-actualización-eliminación) con él.

[!code-csharp[Main](data-storage-options/samples/sample3.cs)]

![Tabla FixItTasks](data-storage-options/_static/image15.png)

La aplicación repararlo incluye una interfaz de repositorio que utiliza para las operaciones CRUD trabajar con el almacén de datos.

[!code-csharp[Main](data-storage-options/samples/sample4.cs)]

Tenga en cuenta que los métodos de repositorio son todos los asincrónico, lo que todo acceso a datos puede realizarse de forma asincrónica por completo.

La implementación de repositorio llama a métodos asincrónicos de Entity Framework para trabajar con los datos, como consultas LINQ, así como para insertar, actualiza y elimina operaciones. Este es un ejemplo del código para buscar una tarea repararlo.

[!code-csharp[Main](data-storage-options/samples/sample5.cs)]

Observará también hay algún tiempo y el código de registro de error aquí, veremos que más adelante en el [capítulo de supervisión y telemetría](monitoring-and-telemetry.md).

<a id="sqliaas"></a>
## <a name="choosing-sql-database-paas-versus-sql-server-in-a-vm-iaas-in-azure"></a>Elección de base de datos SQL (PaaS) frente a SQL Server en una máquina virtual (IaaS) de Azure

Un aspecto interesante acerca de SQL Server y base de datos de SQL Azure es que el modelo de programación principal para ambos es idéntico. Puede utilizar la mayoría de las mismas habilidades en ambos entornos. Incluso puede utilizar una base de datos de SQL Server en el desarrollo y una instancia de base de datos SQL en la nube, que es cómo se configura la aplicación repararlo.

Como alternativa, puede ejecutar el mismo servidor de SQL Server en la nube que ejecutan de forma local mediante la instalación en máquinas virtuales de IaaS. Para algunas aplicaciones heredadas, ejecuta SQL Server en una máquina virtual podría ser una mejor solución. Dado que una base de datos de SQL Server se ejecuta en una máquina virtual dedicada, tiene más recursos a su disposición que una base de datos de la base de datos SQL que se ejecuta en un servidor compartido. Esto significa que una base de datos de SQL Server puede ser más grande y todavía funciona correctamente. En general, cuanto menor sea el tamaño de la base de datos y el tamaño de la tabla, el mejor el caso de uso funciona para la base de datos de SQL (PaaS).

Estas son algunas directrices sobre cómo elegir entre los dos modelos.

| Base de datos SQL Azure (PaaS) | SQL Server en una máquina Virtual (IaaS) |
| --- | --- |
| **Los profesionales de TI** -no tiene que crear o administrar las máquinas virtuales, actualizar o revisión de SO o SQL; Azure hace automáticamente. -Alta disponibilidad integrada, con un SLA de nivel de base de datos. -Bajo costo total de propiedad (TCO) porque solo se paga por lo que se usa (no se necesita ninguna licencia). -Adecuado para administrar un gran número de bases de datos menores (&lt;= 500 GB). -Fáciles de crear dinámicamente nuevas bases de datos para habilitar el escalado horizontal. | ***Los profesionales de TI*** : característica compatible con el servidor local de SQL. -Puede implementar SQL Server [alta disponibilidad mediante AlwaysOn](https://www.microsoft.com/sqlserver/solutions-technologies/mission-critical-operations/high-availability.aspx) en 2 + máquinas virtuales, con el SLA de nivel de máquina virtual. -Tiene un control completo sobre la administración de SQL. -Puede reutilizar las licencias SQL que ya posea o paga por horas para una. -Muy útil para tratar menos pero más grandes (1 TB +) bases de datos. |
| **Inconvenientes** -algunas características huecos en comparación con el servidor local de SQL (falta de [integración CLR](https://technet.microsoft.com/library/ms131102.aspx), [TDE](https://technet.microsoft.com/library/bb934049.aspx), [compatibilidad de compresión](https://technet.microsoft.com/library/cc280449.aspx), [SQL Server Reporting Services](https://technet.microsoft.com/library/ms159106.aspx), etc.)-límite de tamaño de base de datos de 500 GB. | ***Inconvenientes*** : las actualizaciones o revisiones (sistema operativo y SQL) son responsabilidad suya: creación y administración de bases de datos son responsabilidad suya - IOPS de disco (operaciones de entrada/salida por segundo) limitada a entre 8000 (a través de unidades de 16 datos). |

Si desea usar SQL Server en una máquina virtual, puede usar su propia licencia de SQL Server, o puede pagar para uno por horas. Por ejemplo, en el portal o a través de la API de REST puede crear una nueva máquina virtual con una imagen de SQL Server.

![Crear máquinas virtuales con SQL Server](data-storage-options/_static/image16.png)

![Lista de imágenes de VM de SQL Server](data-storage-options/_static/image17.png)

Cuando se crea una máquina virtual con una imagen de SQL Server, se tasa Pro el costo de la licencia de SQL Server por horas según el uso de la máquina virtual. Si tiene un proyecto que sólo se va a ejecutar para un par de meses, es costoso pagar por horas. Si cree que el proyecto se va a la última durante años, es más barato comprar la licencia como haría normalmente.

## <a name="summary"></a>Resumen

Cloud computing hace que sea práctico mezclar y enfoques de almacenamiento de datos de coincidencia que mejor satisfacen las necesidades de la aplicación. Si está creando una aplicación nueva, medite concienzudamente sobre las preguntas que se muestran aquí para elegir los métodos que seguirán funcionando bien cuando crece la aplicación. El [siguiente capítulo](data-partitioning-strategies.md) se explican algunas estrategias de partición que pueden utilizar para combinar varios enfoques de almacenamiento de datos.

## <a name="resources"></a>Recursos

Para obtener más información, vea los siguientes recursos.

Elegir una plataforma de base de datos:

- [Acceso a datos para las soluciones de nube altamente escalables: usar SQL y NoSQL, persistencia Polyglot](http://aka.ms/dag-doc). Libro electrónico por Microsoft Patterns and Practices que entra en profundidad en los diferentes tipos de datos almacena disponibles para las aplicaciones de nube.
- [Microsoft patrones y prácticas - Guía de Azure](https://msdn.microsoft.com/library/ff898430.aspx). Consulte el manual de la coherencia de datos, la replicación de datos e instrucciones de sincronización, el patrón de tabla del índice, el patrón de materializar la vista.
- [BASE: Alternativa Acid](http://queue.acm.org/detail.cfm?id=1394128). El artículo sobre el equilibrio entre la coherencia de los datos y la escalabilidad.
- [Las bases de siete datos en siete semanas: una guía para las bases de datos modernas y el movimiento de NoSQL](https://www.amazon.com/Seven-Databases-Weeks-Modern-Movement/dp/1934356921). Libro de Eric Redmond y Jim R. Wilson. Se recomienda encarecidamente para introducir usted mismo a la amplia variedad de plataformas de almacenamiento de datos disponibles en la actualidad.

Elegir entre SQL Server y base de datos SQL:

- [Vista previa de Premium para obtener instrucciones de base de datos SQL](https://msdn.microsoft.com/library/windowsazure/dn369873.aspx). Introducción a Premium de base de datos de SQL y orientación sobre cuándo elegir sobre las ediciones de SQL de base de datos Web y Business.
- [Instrucciones y limitaciones (base de datos SQL Azure)](https://msdn.microsoft.com/library/windowsazure/ff394102.aspx). Página del portal que se vincula a la documentación acerca de las limitaciones de la base de datos SQL, incluida una que se centra en características de SQL Server de esa base de datos de SQL no es compatible con.
- [SQL Server en máquinas virtuales de Azure](https://msdn.microsoft.com/library/windowsazure/jj823132.aspx). Página del portal que se vincula a la documentación sobre la ejecución de SQL Server en Azure.
- [Scott Guthrie explica las bases de datos de SQL en Azure](https://azure.microsoft.com/documentation/videos/sql-in-azure-scottgu/). 6 minutos vídeo de introducción a la base de datos de SQL por Scott Guthrie.
- [Patrones de aplicación y las estrategias de desarrollo para SQL Server en máquinas virtuales de Azure](https://msdn.microsoft.com/library/windowsazure/dn574746.aspx).

Uso de Entity Framework y base de datos SQL en una aplicación Web ASP.NET

- [Introducción a 6 de EF mediante MVC 5](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Nueve partes serie de tutoriales que le guía por la creación de una aplicación MVC que usa EF y la base de datos se implementa en Azure y base de datos SQL.
- [Implementación de Web ASP.NET con Visual Studio](../../../../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md). Parte de doce serie de tutoriales que entra en más profundidad acerca de cómo implementar una base de datos mediante EF Code First.
- [Implementar una aplicación de MVC de ASP.NET 5 segura con pertenencia, OAuth y base de datos SQL a un sitio Web de Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/). Tutorial paso a paso que le guía por la creación de una aplicación web que usa la autenticación, almacena las tablas de la aplicación en la base de datos de pertenencia, modifica el esquema de base de datos e implementa la aplicación en Azure.
- [Mapa de contenido de acceso de datos de ASP.NET](https://go.microsoft.com/fwlink/p/?LinkId=282414). Vínculos a recursos para trabajar con EF y base de datos SQL.

Uso de MongoDB en Azure:

- [MongoLab - MongoDB en Azure](http://msopentech.com/opentech-projects/mongolab-mongodb-on-windows-azure/). Página del portal para obtener documentación sobre la ejecución de MongoDB en Azure.
- [Crear un sitio web de Azure que se conecta a MongoDB que se ejecuta en una máquina virtual en Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-store-data-mongodb-vm/). Tutorial paso a paso que muestra cómo utilizar una base de datos de MongoDB en una aplicación web ASP.NET.

HDInsight (Hadoop en Azure):

- [HDInsight](https://azure.microsoft.com/documentation/services/hdinsight/). Portal de documentación de HDInsight en el [Azure](https://azure.microsoft.com/) sitio Web.
- [Hadoop y HDInsight: Big Data en Azure](https://msdn.microsoft.com/magazine/dn385705.aspx). Artículo de MSDN Magazine Bruno Terkaly y Villalobos Ricardo, introducción a Hadoop en Azure.
- [Microsoft patrones y prácticas - Guía de Azure](https://msdn.microsoft.com/library/dn568099.aspx). Vea MapReduce patrón.

> [!div class="step-by-step"]
> [Anterior](single-sign-on.md)
> [Siguiente](data-partitioning-strategies.md)
