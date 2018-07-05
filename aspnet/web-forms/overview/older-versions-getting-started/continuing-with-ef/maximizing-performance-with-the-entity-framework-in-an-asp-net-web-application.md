---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application
title: Maximizar el rendimiento con Entity Framework 4.0 en una aplicación de ASP.NET 4 Web | Microsoft Docs
author: tdykstra
description: Esta serie de tutoriales se basa en la aplicación web de Contoso University establecen que se crea mediante la introducción a la serie de tutoriales de Entity Framework 4.0. PUEDO...
ms.author: aspnetcontent
ms.date: 01/26/2011
ms.assetid: 4e43455e-dfa1-42db-83cb-c987703f04b5
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application
msc.type: authoredcontent
ms.openlocfilehash: 2c5aa6c8a512ccd2ff4b94f5a8a60b31aaa39fc5
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37828285"
---
<a name="maximizing-performance-with-the-entity-framework-40-in-an-aspnet-4-web-application"></a>Maximizar el rendimiento con Entity Framework 4.0 en una aplicación Web 4 de ASP.NET
====================
por [Tom Dykstra](https://github.com/tdykstra)

> Esta serie de tutoriales se basa en la aplicación web de Contoso University creado por el [Introducción a Entity Framework 4.0](https://asp.net/entity-framework/tutorials#Getting%20Started) serie de tutoriales. Si no has completado los tutoriales anteriores, como un punto de partida para este tutorial puede [descargar la aplicación](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) que puede haberla creado. También puede [descargar la aplicación](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) creado por la serie de tutoriales completa. Si tiene preguntas acerca de los tutoriales, puede publicarlos en el [foro de ASP.NET Entity Framework](https://forums.asp.net/1227.aspx).


En el tutorial anterior, vimos cómo controlar los conflictos de simultaneidad. Este tutorial muestra opciones para mejorar el rendimiento de una aplicación web ASP.NET que usa Entity Framework. Obtendrá información sobre varios métodos para maximizar el rendimiento o para diagnosticar problemas de rendimiento.

Información que se presenta en las secciones siguientes es probable que sea útil en una amplia variedad de escenarios:

- Cargar eficazmente datos relacionados.
- Administrar el estado de vista.

Información presentada en las secciones siguientes puede ser útil si tiene consultas individuales que presente problemas de rendimiento:

- Use el `NoTracking` merge (opción).
- Compilación previa de las consultas LINQ.
- Examine los comandos de consulta que se envían a la base de datos.

Información que se presenta en la sección siguiente es potencialmente útil para las aplicaciones que tienen modelos de datos extremadamente grandes:

- Generar previamente las vistas.

> [!NOTE]
> Rendimiento de la aplicación Web se ve afectado por diversos factores, incluidos aspectos como el tamaño de los datos de solicitud y respuesta, la velocidad de las consultas de base de datos, cuántas solicitudes que puede poner en cola el servidor y la rapidez con que puede dar servicio ellos e incluso la eficacia de cualquiera bibliotecas de script de cliente puede que esté usando. Si el rendimiento es fundamental en su aplicación, o si las pruebas o la experiencia muestra que el rendimiento de la aplicación no es satisfactorio, debe seguir un protocolo normal para ajustar el rendimiento. Medición para determinar dónde se producen cuellos de botella de rendimiento y, a continuación, solucione las áreas que tendrán el mayor impacto en el rendimiento general de la aplicación.
> 
> En este tema se centra principalmente en formas en que potencialmente se puede mejorar el rendimiento específicamente de Entity Framework en ASP.NET. Las sugerencias aquí son útiles si determina que el acceso a datos es uno de los cuellos de botella de rendimiento en la aplicación. Excepto como se indicó, los métodos que se explican aquí no deben interpretarse como &quot;procedimientos recomendados&quot; en general, muchos de ellos son adecuados solo en situaciones excepcionales o a tipos muy específicos de dirección de los cuellos de botella de rendimiento.


Para iniciar el tutorial, inicie Visual Studio y abra la aplicación web de Contoso University establecen que estaba trabajando con en el tutorial anterior.

## <a name="efficiently-loading-related-data"></a>Carga de forma eficaz los datos relacionados

Hay varias maneras de que Entity Framework puede cargar datos relacionados en las propiedades de navegación de una entidad:

- *Carga diferida*. Cuando la entidad se lee por primera vez, no se recuperan datos relacionados. Pero la primera vez que intente obtener acceso a una propiedad de navegación, se recuperan automáticamente los datos necesarios para esa propiedad de navegación. Esto da como resultado varias consultas que se envían a la base de datos: uno para la propia entidad y otro cada vez que los datos de la entidad relacionados que se debe recuperar. 

    [![Image05](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image2.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image1.png)

*Carga diligente*. Cuando se lee la entidad, junto a ella se recuperan datos relacionados. Esto normalmente da como resultado una única consulta de combinación en la que se recuperan todos los datos que se necesitan. Carga diligente se especifica mediante el uso de la `Include` método, como ya hemos visto en estos tutoriales.

[![Image07](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image4.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image3.png)

- *Carga explícita*. Esto es similar a la carga diferida, salvo que recupera explícitamente los datos relacionados en el código. no sucede automáticamente cuando tiene acceso a una propiedad de navegación. Cargar datos relacionados manualmente mediante el `Load` método de la propiedad de navegación de colecciones, o bien usar la `Load` método de la propiedad de referencia para las propiedades que contienen un solo objeto. (Por ejemplo, se llama a la `PersonReference.Load` método para cargar el `Person` propiedad de navegación de un `Department` entity.)

    [![Image06](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image6.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image5.png)

Porque no recuperan inmediatamente los valores de propiedad, la carga diferida y la carga explícita son ambos conocen también como *la carga diferida*.

La carga diferida es el comportamiento predeterminado para un contexto del objeto que se ha generado por el diseñador. Si abre el *SchoolModel.Designer.cs* archivo que define la clase de contexto de objeto, encontrará tres métodos de constructor y cada uno de ellos incluye la siguiente instrucción:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample1.cs)]

En general, si sabe que necesita datos relacionados para cada entidad de la carga diligente, recuperada ofrece el mejor rendimiento, porque una consulta única enviada a la base de datos es normalmente más eficaz que consultas independientes para cada entidad recuperada. Por otro lado, si necesita tener acceso a las propiedades de navegación de la entidad sólo con poca frecuencia o solo para un pequeño conjunto de entidades, la carga diferida o la carga explícita puede ser más eficaz, ya que la carga diligente recuperaría más datos de los que necesita.

En una aplicación web, la carga diferida puede ser relativamente poco valor de todos modos, dado que las acciones del usuario que afectan a la necesidad de datos relacionadas tienen lugar en el explorador, que no tiene conexión con el contexto del objeto que representa la página. Por otro lado, al enlazar un control, normalmente saber qué datos necesita y, por lo que normalmente es mejor para elegir la carga diligente o según la carga aplazada lo que es adecuado para cada escenario.

Además, un control enlazado a datos podría usar un objeto de entidad después de desecha el contexto del objeto. En ese caso, una carga diferida una propiedad de navegación se produciría un error. Está claro que recibe el mensaje de error: &quot;`The ObjectContext instance has been disposed and can no longer be used for operations that require a connection.`&quot;

El `EntityDataSource` control deshabilita la carga diferida de manera predeterminada. Para el `ObjectDataSource` control que está usando para el tutorial actual (o si tener acceso al contexto de objeto desde el código de página), hay varias maneras de hacer que diferida carga deshabilitada de forma predeterminada. Se puede deshabilitar al crear instancias de un contexto del objeto. Por ejemplo, puede agregar la línea siguiente al método constructor de la `SchoolRepository` clase:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample2.cs)]

Para la aplicación Contoso University, hará que el contexto del objeto deshabilitar automáticamente para que esta propiedad no tiene que establecerse cada vez que se crea una instancia de un contexto de la carga diferida.

Abra el *SchoolModel.edmx* modelo de datos, haga clic en la superficie de diseño y, a continuación, en el panel Propiedades, establezca la **carga diferida habilitada** propiedad `False`. Guarde y cierre el modelo de datos.

[![Image04](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image8.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image7.png)

## <a name="managing-view-state"></a>Administrar el estado de vista

Con el fin de proporcionar funcionalidad de actualización, una página web ASP.NET debe almacenar los valores de propiedad originales de una entidad cuando se procesa una página. Durante el procesamiento del control de devolución de datos puede volver a crear el estado original de la entidad y llamar a la entidad `Attach` método antes de aplicar los cambios y llamar a la `SaveChanges` método. De forma predeterminada, los controles de datos de formularios Web Forms ASP.NET usan el estado de vista para almacenar los valores originales. Sin embargo, el estado de vista puede afectar al rendimiento, porque está almacenada en los campos ocultos que pueden aumentar considerablemente el tamaño de la página que se envía a y desde el explorador.

Técnicas para administrar el estado de vista ni otras alternativas, como el estado de sesión, no son exclusivas de Entity Framework, por lo que este tutorial no entrar en este tema con todo detalle. Para obtener más información, consulte los vínculos al final del tutorial.

Sin embargo, la versión 4 de ASP.NET proporciona una nueva forma de trabajar con el estado de vista que deben tener en cuenta todos los desarrolladores de aplicaciones de formularios Web Forms de ASP.NET: el `ViewStateMode` propiedad. Esta nueva propiedad se puede establecer en el nivel de página o control, y le permite deshabilitar el estado de vista predeterminada para una página y habilitarlo solo para los controles que lo necesiten.

Para las aplicaciones donde el rendimiento es crítico, una buena práctica es siempre se deshabilita el estado de vista en el nivel de página y habilitarlo solo para los controles que lo requieran. No se puede reducir considerablemente el tamaño del estado de vista en las páginas de Contoso University por este método, pero para ver cómo funciona, deberá hacerlo el *Instructors.aspx* página. Que contiene muchos controles, incluido un `Label` control que tenga deshabilitado. Ninguno de los controles de esta página realmente debe tener habilitado el estado de vista. (El `DataKeyNames` propiedad de la `GridView` control especifica el estado que se debe mantener entre devoluciones de datos, pero estos valores se mantienen en estado de control, lo que no se ve afectado por la `ViewStateMode` propiedad.)

El `Page` directiva y `Label` marcado del control actualmente es similar al siguiente:

[!code-aspx[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample3.aspx)]

Realice los cambios siguientes:

- Agregar `ViewStateMode="Disabled"` a la `Page` directiva.
- Quitar `ViewStateMode="Disabled"` desde el `Label` control.

El marcado ahora es similar al siguiente:

[!code-aspx[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample4.aspx)]

Ahora está deshabilitado para todos los controles. Si agrega un control que se debe usar el estado de vista más adelante, todo lo que necesita hacer es incluir el `ViewStateMode="Enabled"` atributo para ese control.

## <a name="using-the-notracking-merge-option"></a>Con la opción de combinación NoTracking

Cuando un contexto del objeto recupera filas de la base de datos y crea objetos de entidad que representan a ellos, de forma predeterminada también realiza un seguimiento de esos objetos de entidad con su administrador de estado de objeto. Estos datos de seguimiento actúan como una memoria caché y se utilizan cuando se actualiza una entidad. Dado que una aplicación web normalmente tiene instancias de contexto de objetos de corta duración, las consultas a menudo devuelvan datos que no deben realizar un seguimiento, porque el contexto del objeto que hace que se eliminará antes de cualquiera de las entidades que lee se vuelven a utilizar o actualizan.

En Entity Framework, puede especificar si el contexto del objeto realiza el seguimiento de los objetos entidad estableciendo un *merge (opción)*. Puede establecer la opción de combinación para consultas individuales o conjuntos de entidades. Si se establece para un conjunto de entidades, que significa que está configurando la opción de combinación predeterminada para todas las consultas que se crean para ese conjunto de entidades.

Para la aplicación Contoso University, el seguimiento no es necesario para cualquiera de los conjuntos de entidades que tiene acceso desde el repositorio, por lo que puede establecer la opción de combinación `NoTracking` para esos conjuntos de entidades al crear instancias de contexto del objeto en la clase del repositorio. (Tenga en cuenta que en este tutorial, establecer la opción de combinación no tendrá un efecto notable en el rendimiento de la aplicación. El `NoTracking` opción es probable que sea una mejora del rendimiento observable solo en determinados escenarios de alto volumen de datos.)

En la carpeta de la capa DAL, abra el *SchoolRepository.cs* archivo y agregue un método de constructor que establece la opción de combinación para los conjuntos de entidades que se tiene acceso al repositorio:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample5.cs)]

## <a name="pre-compiling-linq-queries"></a>Compilar previamente las consultas LINQ

La primera vez que Entity Framework se ejecuta una consulta de Entity SQL durante la vida de un determinado `ObjectContext` instancia, tarda algún tiempo para compilar la consulta. El resultado de la compilación se almacena en caché, lo que significa que las ejecuciones posteriores de la consulta son mucho más rápidas. Las consultas LINQ siguen un patrón similar, salvo que parte del trabajo necesario para compilar la consulta se realiza cada vez que se ejecuta la consulta. En otras palabras, para las consultas LINQ, de forma predeterminada todos los resultados de la compilación no se almacenan en caché.

Si tiene una consulta LINQ que se espera que se ejecute varias veces en la vida de un contexto del objeto, puede escribir código que hace que todos los resultados de compilación en la memoria caché la primera vez que se ejecuta la consulta LINQ.

Como ilustración, deberá hacerlo para dos `Get` métodos en el `SchoolRepository` (clase), uno de los cuales no toma ningún parámetro (el `GetInstructorNames` método) y otro que requieren un parámetro (el `GetDepartmentsByAdministrator` método). Estos métodos como representan ahora realmente no es necesario al compilarse porque no son las consultas LINQ:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample6.cs)]

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample7.cs)]

Sin embargo, por lo que puede probar las consultas compiladas, podrá continuar como si éstos se hubiese escritos como las siguientes consultas LINQ:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample8.cs)]

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample9.cs)]

Puede cambiar el código de estos métodos a lo que se ha mostrado anteriormente y ejecute la aplicación para comprobar que funciona antes de continuar. Pero las siguientes instrucciones comenzar de inmediato en la creación de versiones compiladas previamente de ellos.

Cree un archivo de clase en el *DAL* carpeta, asígnele el nombre *SchoolEntities.cs*y reemplace el código existente por el código siguiente:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample10.cs)]

Este código crea una clase parcial que extiende la clase de contexto de objeto generado automáticamente. La clase parcial incluye dos consultas LINQ compiladas con la `Compile` método de la `CompiledQuery` clase. También crea los métodos que puede usar para llamar a las consultas. Guarde y cierre este archivo.

A continuación, en *SchoolRepository.cs*, cambiar las existentes `GetInstructorNames` y `GetDepartmentsByAdministrator` métodos en el repositorio de clase para que llamen a las consultas compiladas:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample11.cs)]

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample12.cs)]

Ejecute el *Departments.aspx* página para comprobar que funciona igual que antes. El `GetInstructorNames` se llama al método con el fin de rellenar la lista desplegable de administrador y el `GetDepartmentsByAdministrator` se llama al método al hacer clic en **actualización** con el fin de comprobar que ningún instructor es un administrador de más de uno departamento.

[![Image03](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image10.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image9.png)

Dispone de las consultas precompiladas en la aplicación Contoso University solo para ver cómo hacerlo, no porque un modo contrastable podría mejorar el rendimiento. Compilar previamente las consultas LINQ agrega un nivel de complejidad en el código, por tanto, asegúrese de que hacerlo solo para las consultas que representan realmente los cuellos de botella en la aplicación.

## <a name="examining-queries-sent-to-the-database"></a>Examen de las consultas enviadas a la base de datos

Cuando se está investigando problemas de rendimiento, a veces resulta útil saber los comandos SQL exactos que Entity Framework está enviando a la base de datos. Si está trabajando con un `IQueryable` objeto, una manera de hacerlo es usar el `ToTraceString` método.

En *SchoolRepository.cs*, cambie el código en el `GetDepartmentsByName` método para que coincida con el ejemplo siguiente:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample13.cs)]

El `departments` variable se debe convertir en un `ObjectQuery` tipo sólo porque la `Where` método al final de la línea anterior crea un `IQueryable` objeto; sin la `Where` método, la conversión no sería necesaria.

Establecer un punto de interrupción en el `return` de línea y, a continuación, ejecute el *Departments.aspx* página en el depurador. Cuando se alcance el punto de interrupción, examinar el `commandText` variable en el **variables locales** ventana y use el visualizador de texto (el icono de lupa en la **valor** columna) para mostrar su valor en el **Visualizador de texto** ventana. Puede ver todo el comando SQL que se origina este código:

[![Image08](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image12.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image11.png)

Como alternativa, la característica de IntelliTrace en Visual Studio Ultimate proporciona una manera de ver los comandos SQL generados por Entity Framework que no requiere cambiar el código o incluso establecer un punto de interrupción.

> [!NOTE]
> Puede realizar los procedimientos siguientes solo si tiene Visual Studio Ultimate.


Restaurar el código original de la `GetDepartmentsByName` método y, a continuación, ejecute el *Departments.aspx* página en el depurador.

En Visual Studio, seleccione el **depurar** menú y, a continuación, **IntelliTrace**y, a continuación, **eventos de IntelliTrace**.

[![Image11](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image14.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image13.png)

En el **IntelliTrace** ventana, haga clic en **interrumpir todos**.

[![Image12](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image16.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image15.png)

El **IntelliTrace** ventana muestra una lista de eventos recientes:

[![Image09](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image18.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image17.png)

Haga clic en el **ADO.NET** línea. Se expande para mostrar el texto del comando:

[![Image10](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image20.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image19.png)

Puede copiar la cadena de texto de comando completo en el Portapapeles desde el **variables locales** ventana.

Supongamos que estaba trabajando con una base de datos con varias tablas, relaciones y columnas que la simple `School` base de datos. Es posible que una consulta que recopila toda la información que necesita en un único `Select` instrucción que contenga múltiples `Join` cláusulas se vuelve demasiado complejo trabajar de forma eficaz. En ese caso puede cambiar de diligente a la carga explícita para simplificar la consulta.

Por ejemplo, pruebe a cambiar el código en el `GetDepartmentsByName` método *SchoolRepository.cs*. Actualmente método ya que dispone de una consulta de objeto que tiene `Include` métodos para la `Person` y `Courses` las propiedades de navegación. Reemplace el `return` instrucción con el código que realiza la carga explícita, tal como se muestra en el ejemplo siguiente:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample14.cs)]

Ejecute el *Departments.aspx* página en el depurador y compruebe el **IntelliTrace** ventana nuevo como se hizo antes. Ahora, cuando había una sola consulta antes, verá una secuencia larga de ellos.

[![Image13](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image22.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image21.png)

Haga clic en la primera **ADO.NET** visualizado de línea para ver qué ha pasado con la consulta compleja antes.

[![Image14](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image24.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image23.png)

La consulta de los departamentos de TI se ha convertido en un sencillo `Select` de consulta sin ningún `Join` cláusula, pero va seguido de distintas consultas que recuperan cursos relacionados y un administrador, mediante un conjunto de dos consultas para cada departamento devuelto por el original consulta.

> [!NOTE]
> Si deja diferida habilitado, el patrón que se observa aquí, con la misma consulta que se repiten muchas veces, la carga podría deberse a la carga diferida. Un patrón que normalmente se desea evitar es carga diferida datos relacionados para cada fila de la tabla principal. A menos que haya comprobado que una consulta de combinación solo es demasiado compleja para ser eficaz, normalmente podrá mejorar el rendimiento en estos casos, cambie la consulta principal para usar la carga diligente.


## <a name="pre-generating-views"></a>Generar vistas previamente

Cuando un `ObjectContext` en primer lugar se crea el objeto en un dominio de aplicación, Entity Framework genera un conjunto de clases que utiliza para tener acceso a la base de datos. Estas clases se denominan *vistas*, y si tiene un modelo de datos muy grandes, generar estas vistas puede retrasar la respuesta de la carpeta del sitio web a la primera solicitud para una página después de Inicializa un nuevo dominio de aplicación. Puede reducir este retraso de la primera solicitud mediante la creación de las vistas en tiempo de compilación en lugar de en tiempo de ejecución.

> [!NOTE]
> Si la aplicación no tiene un modelo de datos extremadamente grandes, o si tiene un modelo de datos de gran tamaño, pero no le preocupa un problema de rendimiento que afecta a la primera solicitud de página después de que IIS se recicla, puede omitir esta sección. Vista de creación no ocurre cada vez que crea instancias de un `ObjectContext` de objeto, dado que las vistas se almacenan en caché en el dominio de aplicación. Por lo tanto, a menos que con frecuencia se recicla la aplicación en IIS, muy pocas solicitudes de página se beneficiarían de las vistas generadas previamente.


Puede generar previamente vistas mediante el *EdmGen.exe* herramienta de línea de comandos o mediante un *Kit de herramientas de transformación de plantillas de texto* plantilla (T4). En este tutorial usará una plantilla T4.

En el *DAL* carpeta, agregue un archivo mediante el **plantilla de texto** plantilla (se encuentra en la **General** nodo en el **plantillas instaladas** lista), y asígnele el nombre *SchoolModel.Views.tt*. Reemplace el código existente en el archivo con el código siguiente:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample15.cs)]

Este código genera vistas para una *.edmx* archivo que se encuentra en la misma carpeta que la plantilla y que tiene el mismo nombre que el archivo de plantilla. Por ejemplo, si su archivo de plantilla se denomina *SchoolModel.Views.tt*, buscará un archivo de modelo de datos denominado *SchoolModel.edmx*.

Guarde el archivo y, después, haga clic en el archivo en **el Explorador de soluciones** y seleccione **ejecutar herramienta personalizada**.

[![Image02](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image26.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image25.png)

Visual Studio genera un archivo de código que crea las vistas, que se denomina *SchoolModel.Views.cs* basado en la plantilla. (Es posible que haya observado que el archivo de código se genera antes de seleccionar **ejecutar herramienta personalizada**, tan pronto como guarde el archivo de plantilla.)

[![Image01](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image28.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image27.png)

Ahora puede ejecutar la aplicación y comprobar que funciona igual que antes.

Para obtener más información acerca de las vistas generadas previamente, consulte los siguientes recursos:

- [Cómo: generar previamente vistas para mejorar el rendimiento de las consultas](https://msdn.microsoft.com/library/bb896240.aspx) en el sitio web MSDN. Explica cómo utilizar el `EdmGen.exe` herramienta de línea de comandos para generar previamente las vistas.
- [Aislar Performance with Precompiled/Pre-generated Views en Entity Framework 4](https://blogs.msdn.com/b/appfabriccat/archive/2010/08/06/isolating-performance-with-precompiled-pre-generated-views-in-the-entity-framework-4.aspx) en el blog del equipo de asesoría de clientes de Windows Server AppFabric.

Con esto finaliza la introducción para mejorar el rendimiento en una aplicación web ASP.NET que usa Entity Framework. Para obtener más información, vea los siguientes recursos:

- [Consideraciones de rendimiento (Entity Framework)](https://msdn.microsoft.com/library/cc853327.aspx) en el sitio web MSDN.
- [Relacionados con el rendimiento de publicaciones del blog del equipo de Entity Framework](https://blogs.msdn.com/b/adonet/archive/tags/performance/).
- [EF opciones de combinación y consultas compiladas](https://blogs.msdn.com/b/dsimmons/archive/2010/01/12/ef-merge-options-and-compiled-queries.aspx). Entrada de blog que explica los comportamientos inesperados de las consultas compiladas y mezcla opciones tales como `NoTracking`. Si tiene previsto usar consultas compiladas o manipular la configuración de la opción de combinación en la aplicación, lea esto primero.
- [Entidad relacionada con el marco de trabajo publicaciones en el blog de datos y modelado de Customer Advisory Team](https://blogs.msdn.com/b/dmcat/archive/tags/entity+framework/). Incluye publicaciones en las consultas compiladas y el uso de la Profiler de 2010 Visual Studio para detectar problemas de rendimiento.
- [Conversación de foro de Entity Framework con consejos sobre cómo mejorar el rendimiento de las consultas de gran complejidad](https://social.msdn.microsoft.com/Forums/adodotnetentityframework/thread/ffe8b2ab-c5b5-4331-8988-33a872d0b5f6).
- [Recomendaciones de administración de estado de ASP.NET](https://msdn.microsoft.com/library/z1hkazw7.aspx).
- [Uso de Entity Framework y el origen ObjectDataSource: paginación personalizada](http://geekswithblogs.net/Frez/articles/using-the-entity-framework-and-the-objectdatasource-custom-paging.aspx). Entrada de blog que se basa en la aplicación de ContosoUniversity creada en estos tutoriales para explicar cómo implementar la paginación en el *Departments.aspx* página.

El siguiente tutorial revisa las importantes mejoras a Entity Framework que son nuevas en la versión 4.

> [!div class="step-by-step"]
> [Anterior](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md)
> [Siguiente](what-s-new-in-the-entity-framework-4.md)
