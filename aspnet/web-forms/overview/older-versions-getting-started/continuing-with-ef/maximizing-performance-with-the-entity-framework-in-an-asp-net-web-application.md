---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application
title: Maximizar el rendimiento con Entity Framework 4.0 en una aplicación de ASP.NET 4 Web | Documentos de Microsoft
author: tdykstra
description: Esta serie de tutoriales se basa en la aplicación web de la Universidad de Contoso que se crea mediante la introducción a la serie de tutoriales de Entity Framework 4.0. I...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2011
ms.topic: article
ms.assetid: 4e43455e-dfa1-42db-83cb-c987703f04b5
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application
msc.type: authoredcontent
ms.openlocfilehash: b85645eebf2822b33df944692736ea9d9b69b9aa
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="maximizing-performance-with-the-entity-framework-40-in-an-aspnet-4-web-application"></a>Maximizar el rendimiento con Entity Framework 4.0 en una aplicación Web 4 de ASP.NET
====================
por [Tom Dykstra](https://github.com/tdykstra)

> Esta serie de tutoriales que se basa en la aplicación web de la Universidad de Contoso que se crea mediante la [Getting Started with the Entity Framework 4.0](https://asp.net/entity-framework/tutorials#Getting%20Started) serie de tutoriales. Si no has completado los tutoriales anteriores, como punto de partida para este tutorial puede [descargar la aplicación](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) que puede haberla creado. También puede [descargar la aplicación](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) creado por la serie de tutoriales completa. Si tiene alguna pregunta acerca de los tutoriales, puede publicar para la [foro de ASP.NET Entity Framework](https://forums.asp.net/1227.aspx).


En el tutorial anterior, ha visto cómo tratar los conflictos de simultaneidad. Este tutorial muestra opciones para mejorar el rendimiento de una aplicación web ASP.NET que utiliza Entity Framework. Obtendrá información sobre varios métodos para maximizar el rendimiento o para diagnosticar problemas de rendimiento.

Información que se presenta en las secciones siguientes es probable que sea útil en una amplia variedad de escenarios:

- Cargar eficazmente datos relacionados.
- Administrar el estado de vista.

Información presentada en las secciones siguientes puede ser útil si tiene consultas individuales que presentan problemas de rendimiento:

- Use la `NoTracking` merge (opción).
- Precompilar consultas LINQ.
- Examine los comandos de consulta que se envía a la base de datos.

Información que se presenta en la sección siguiente es potencialmente útil para las aplicaciones que tienen modelos de datos extremadamente grandes:

- Generar previamente las vistas.

> [!NOTE]
> Rendimiento de la aplicación Web se ve afectado por diversos factores, incluidos elementos tales como el tamaño de los datos de solicitud y respuesta, la velocidad de las consultas de base de datos, la cantidad de solicitudes que el servidor puede poner en cola y la rapidez con pueda dar servicio a ellos así como incluso la eficacia de cualquier bibliotecas de script de cliente puede que esté usando. Si el rendimiento resulta esencial en la aplicación, o pruebas o la experiencia muestra que el rendimiento de la aplicación no es satisfactorio, debe seguir protocolo normal para la optimización de rendimiento. Para determinar dónde se producen cuellos de botella de rendimiento de medida y, a continuación, solucione las áreas que tendrán mayor impacto en el rendimiento general de la aplicación.
> 
> En este tema se centra principalmente en las formas en que puede mejorar potencialmente el rendimiento específicamente de Entity Framework en ASP.NET. Las sugerencias aquí son útiles si determina que el acceso a los datos es uno de los cuellos de botella de rendimiento en la aplicación. Excepto como se indicó, no deben interpretarse como los métodos explicados aquí &quot;prácticas recomendadas&quot; en general, muchos de ellos son adecuados solo en situaciones excepcionales o a tipos muy específicos de dirección de los cuellos de botella de rendimiento.


Para iniciar el tutorial, inicie Visual Studio y abra la aplicación web de la Universidad de Contoso que estaba trabajando con en el tutorial anterior.

## <a name="efficiently-loading-related-data"></a>Cargar eficazmente datos relacionados

Hay varias maneras que Entity Framework pueda cargar los datos relacionados en las propiedades de navegación de una entidad:

- *Carga diferida*. Cuando la entidad se lee por primera vez, no se recuperan datos relacionados. Pero la primera vez que intente obtener acceso a una propiedad de navegación, se recuperan automáticamente los datos necesarios para esa propiedad de navegación. Esto da como resultado varias consultas que se envían a la base de datos: uno para la propia entidad y otro se debe recuperar cada vez que los datos de la entidad relacionados. 

    [![Image05](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image2.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image1.png)

*Carga diligente*. Cuando se lee la entidad, junto a ella se recuperan datos relacionados. Esto normalmente da como resultado una única consulta de combinación en la que se recuperan todos los datos que se necesitan. Carga diligente se especifica mediante el `Include` método, como se ha visto ya en estos tutoriales.

[![Image07](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image4.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image3.png)

- *Carga explícita*. Esto es similar a la carga diferida, salvo que explícitamente recuperar los datos relacionados en el código. no sucede automáticamente cuando tiene acceso a una propiedad de navegación. Cargar datos relacionados manualmente mediante la `Load` método de la propiedad de navegación para colecciones, o utilizar el `Load` método de la propiedad de referencia para las propiedades que contienen un solo objeto. (Por ejemplo, se llama a la `PersonReference.Load` método para cargar el `Person` propiedad de navegación de un `Department` entidad.)

    [![Image06](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image6.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image5.png)

Porque no recuperan inmediatamente los valores de propiedad, carga diferida y carga explícita también se las conoce como *carga aplazada*.

Carga diferida es el comportamiento predeterminado para un contexto del objeto que se ha generado por el diseñador. Si abre el *SchoolModel.Designer.cs* archivo que define la clase de contexto de objeto, encontrará tres métodos de constructor y cada uno de ellos incluye la siguiente instrucción:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample1.cs)]

En general, si sabe que necesita datos relacionados para cada entidad carga diligente, recuperada ofrece el mejor rendimiento, porque una sola consulta que se envía a la base de datos es normalmente más eficaz que las consultas independientes para cada entidad recuperados. Por otro lado, si necesita tener acceso a propiedades de navegación de una entidad usen con poca frecuencia o solo para un pequeño conjunto de las entidades, la carga diferida o la carga explícita puede ser más eficaz, porque la carga diligente recuperaría más datos de los que necesita.

En una aplicación web, la carga diferida puede ser de relativamente poco valor de todos modos, porque tienen lugar en el explorador, que no tiene conexión con el contexto del objeto que representa la página de acciones de usuario que afectan a la necesidad de datos relacionados. Por otro lado, al enlazar un control, normalmente saber qué datos hay que necesita y, por lo que normalmente es mejor para elegir carga diligente o la carga aplazada en función de lo que es adecuado para cada escenario.

Además, un control enlazado a datos podría usar un objeto de entidad después de que se elimine el contexto del objeto. En ese caso, se producirá un error en un intento de carga diferida una propiedad de navegación. El mensaje de error que recibe es claro: &quot;`The ObjectContext instance has been disposed and can no longer be used for operations that require a connection.`&quot;

El `EntityDataSource` control deshabilita la carga diferida de forma predeterminada. Para el `ObjectDataSource` control que está usando para el tutorial actual (o si tiene acceso al contexto del objeto de código de la página), hay varias maneras de hacer que sea diferida carga deshabilitada de forma predeterminada. Se puede deshabilitar cuando cree instancias de un contexto del objeto. Por ejemplo, puede agregar la línea siguiente al método de constructor de la `SchoolRepository` clase:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample2.cs)]

Para la aplicación de la Universidad de Contoso, hará que el contexto del objeto deshabilitar automáticamente la carga diferida para que esta propiedad no tiene que establecerse siempre que se crea una instancia de un contexto.

Abra la *SchoolModel.edmx* datos de modelo, haga clic en la superficie de diseño y, a continuación, en el panel Propiedades, establezca la **carga diferida habilitado** propiedad `False`. Guarde y cierre el modelo de datos.

[![Image04](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image8.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image7.png)

## <a name="managing-view-state"></a>Administrar el estado de vista

Con el fin de proporcionar funcionalidad de actualización, una página web ASP.NET debe almacenar los valores originales de la propiedad de una entidad cuando se representa una página. Durante el control de procesamiento de la devolución de llamada puede volver a crear el estado original de la entidad y llamar a la entidad `Attach` método antes de aplicar los cambios y llamar a la `SaveChanges` método. De forma predeterminada, los controles de datos de formularios Web Forms de ASP.NET utilizan el estado de vista para almacenar los valores originales. Sin embargo, el estado de vista puede afectar al rendimiento, porque está almacenada en los campos ocultos que pueden aumentar considerablemente el tamaño de la página que se envía a y desde el explorador.

Técnicas para administrar el estado de vista o alternativas como el estado de sesión no son únicas para Entity Framework, por lo que este tutorial no entraremos en este tema en detalle. Para obtener más información, vea los vínculos al final del tutorial.

Sin embargo, la versión 4 de ASP.NET proporciona una nueva forma de trabajar con el estado de vista que todos los desarrolladores de aplicaciones de formularios Web Forms ASP.NET deben tener en cuenta: el `ViewStateMode` propiedad. Esta nueva propiedad se puede establecer en el nivel de página o un control, y le permite deshabilitar el estado de vista de una página de forma predeterminada y habilitar solo para los controles que lo necesitan.

Para las aplicaciones que el rendimiento sea crucial, una buena práctica es siempre deshabilitar el estado de vista en el nivel de página y habilitar solo para los controles que lo requieran. El tamaño del estado de vista en las páginas de la Universidad de Contoso no reducirse sustancialmente por este método, pero para ver cómo funciona, llevará a cabo el *Instructors.aspx* página. Esta página contiene muchos controles, incluyendo un `Label` control que tiene el estado de vista deshabilitado. Ninguno de los controles de esta página realmente necesita tener habilitado el estado de vista. (El `DataKeyNames` propiedad de la `GridView` control especifica el estado que debe mantenerse entre las devoluciones de datos, pero estos valores se mantienen en estado de control, lo que no se ve afectado por la `ViewStateMode` propiedad.)

El `Page` directiva y `Label` marcado del control actualmente es similar al siguiente:

[!code-aspx[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample3.aspx)]

Realice los cambios siguientes:

- Agregar `ViewStateMode="Disabled"` a la `Page` directiva.
- Quitar `ViewStateMode="Disabled"` desde el `Label` control.

El marcado ahora es similar al siguiente:

[!code-aspx[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample4.aspx)]

Estado de vista ahora está deshabilitado para todos los controles. Si posteriormente, agregar un control que es necesario usar el estado de vista, todo lo que necesita hacer es incluir el `ViewStateMode="Enabled"` atributo de ese control.

## <a name="using-the-notracking-merge-option"></a>Con la opción de combinación NoTracking

Cuando un contexto del objeto recupera las filas de la base de datos y crea objetos de entidad que tienen tipos de archivo, de forma predeterminada también realiza un seguimiento de esos objetos de entidad con su administrador de estado de objeto. Estos datos de seguimiento actúa como una memoria caché y se utilizan cuando se actualiza una entidad. Dado que una aplicación web normalmente tiene instancias de contexto de objetos de corta duración, las consultas a menudo devuelvan datos que no tienen que llevar un seguimiento porque el contexto del objeto que hace que se eliminará antes de cualquiera de las entidades que se lee se vuelven a utilizar o actualizarán.

En Entity Framework, puede especificar si el contexto del objeto realiza el seguimiento de objetos entidad estableciendo un *merge (opción)*. Puede establecer la opción de combinación para las consultas individuales o para los conjuntos de entidades. Si se establece para un conjunto de entidades, que significa que se está estableciendo la opción de combinación predeterminada para todas las consultas que se crean para ese conjunto de entidades.

Para la aplicación de la Universidad de Contoso, seguimiento no se necesita para cualquiera de los conjuntos de entidades que tiene acceso desde el repositorio, por lo que puede establecer la opción de combinación `NoTracking` para los conjuntos de entidades al crear una instancia de contexto del objeto de la clase de repositorio. (Tenga en cuenta que en este tutorial, establecer la opción de combinación no tendrá un efecto notable en el rendimiento de la aplicación. El `NoTracking` opción es probable que una mejora del rendimiento observable solo en determinados escenarios de gran volumen de datos.)

En la carpeta de la capa DAL, abra el *SchoolRepository.cs* archivo y agregar un método de constructor que establece la opción de combinación para la entidad, se establece que tiene acceso el repositorio:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample5.cs)]

## <a name="pre-compiling-linq-queries"></a>Compilación previa de las consultas LINQ

La primera vez que Entity Framework se ejecuta una consulta de Entity SQL durante la vida de un determinado `ObjectContext` instancia, tarda algún tiempo para compilar la consulta. El resultado de la compilación se almacena en caché, lo que significa que las ejecuciones posteriores de la consulta son mucho más rápidas. Las consultas LINQ siguen un patrón similar, salvo que algunas de las tareas necesarias para compilar la consulta se realiza cada vez que se ejecuta la consulta. En otras palabras, para las consultas LINQ, de forma predeterminada todos los resultados de la compilación no se almacenan en caché.

Si tiene una consulta LINQ que se espera que se ejecute varias veces en la vida de un contexto del objeto, puede escribir código que hace que todos los resultados de la compilación en la memoria caché la primera vez que se ejecuta la consulta LINQ.

Como ilustración, deberá hacerlo para dos `Get` métodos en el `SchoolRepository` (clase), uno de los cuales no toma ningún parámetro (el `GetInstructorNames` método) y otro que requieren un parámetro (el `GetDepartmentsByAdministrator` método). Estos métodos como éstos se levantan ahora realmente no deben compilarse porque no son consultas LINQ:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample6.cs)]

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample7.cs)]

Sin embargo, por lo que puede probar las consultas compiladas, podrá continuar como si se hubiese escritos como las siguientes consultas LINQ:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample8.cs)]

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample9.cs)]

Puede cambiar el código de estos métodos por lo que ha mostrado anteriormente y ejecute la aplicación para comprobar que funciona antes de continuar. Pero las siguientes instrucciones comenzar de inmediato en la creación de versiones precompiladas de ellos.

Crear un archivo de clase en el *DAL* carpeta, asígnele el nombre *SchoolEntities.cs*y reemplace el código existente por el código siguiente:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample10.cs)]

Este código crea una clase parcial que extiende la clase de contexto de objeto generado automáticamente. La clase parcial incluye dos consultas LINQ compiladas con la `Compile` método de la `CompiledQuery` clase. También crea los métodos que puede usar para llamar a las consultas. Guarde y cierre este archivo.

Después, en *SchoolRepository.cs*, cambiar las existentes `GetInstructorNames` y `GetDepartmentsByAdministrator` métodos en el repositorio de clase para que llame a las consultas compiladas:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample11.cs)]

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample12.cs)]

Ejecute el *Departments.aspx* página para comprobar que funciona igual que antes. El `GetInstructorNames` método se llama para rellenar la lista desplegable de administrador y la `GetDepartmentsByAdministrator` método se llama al hacer clic en **actualización** para comprobar que ningún instructor es administradora de más de uno departamento.

[![Image03](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image10.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image9.png)

Dispone de las consultas precompiladas en la aplicación de la Universidad de Contoso solo para ver cómo hacerlo, no porque podría mejorar el rendimiento mejorará en gran medida. La compilación previa de las consultas LINQ agregar un nivel de complejidad en el código, así que asegúrese de que hacerlo solo para las consultas que representan realmente los cuellos de botella de rendimiento en la aplicación.

## <a name="examining-queries-sent-to-the-database"></a>Examen de las consultas enviadas a la base de datos

Cuando se está investigando problemas de rendimiento, a veces resulta útil conocer los comandos SQL exactos que Entity Framework está enviando a la base de datos. Si está trabajando con un `IQueryable` objeto, una manera de hacerlo es utilizar el `ToTraceString` método.

En *SchoolRepository.cs*, cambie el código en el `GetDepartmentsByName` método para que coincida con el ejemplo siguiente:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample13.cs)]

El `departments` variable debe convertirse a un `ObjectQuery` tipo solo porque el `Where` método al final de la línea anterior crea un `IQueryable` objeto; sin el `Where` método, la conversión no sería necesaria.

Establecer un punto de interrupción en la `return` de línea y, a continuación, ejecute el *Departments.aspx* página en el depurador. Cuando se alcanza el punto de interrupción, examine la `commandText` variable en el **locales** ventana y usar el visualizador de texto (la lupa en el **valor** columna) para mostrar su valor en el **Visualizador de texto** ventana. Puede ver todo el comando SQL que es el resultado de este código:

[![Image08](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image12.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image11.png)

Como alternativa, la característica de IntelliTrace en Visual Studio Ultimate proporciona una manera de ver los comandos SQL generados por Entity Framework que no requiere cambios en el código o incluso establecer un punto de interrupción.

> [!NOTE]
> Puede realizar los siguientes procedimientos solo si tiene Visual Studio Ultimate.


Restaure el código original en el `GetDepartmentsByName` método y, a continuación, ejecute el *Departments.aspx* página en el depurador.

En Visual Studio, seleccione la **depurar** menú, a continuación, **IntelliTrace**y, a continuación, **eventos de IntelliTrace**.

[![image11](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image14.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image13.png)

En el **IntelliTrace** ventana, haga clic en **interrumpir todos**.

[![Image12](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image16.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image15.png)

El **IntelliTrace** ventana muestra una lista de eventos recientes:

[![Image09](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image18.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image17.png)

Haga clic en el **ADO.NET** línea. Se expande para mostrar el texto del comando:

[![Image10](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image20.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image19.png)

Puede copiar la cadena de texto de comando completo en el Portapapeles desde el **locales** ventana.

Imagine que estaba trabajando con una base de datos con más tablas, relaciones y columnas que la simple `School` base de datos. Es posible que una consulta que reúne toda la información necesite en un único equipo `Select` una instrucción que contiene varios `Join` cláusulas se vuelve demasiado complejo para que funcione de forma eficaz. En ese caso puede cambiar de carga a la carga explícita para simplificar la consulta diligente.

Por ejemplo, pruebe a cambiar el código en el `GetDepartmentsByName` método *SchoolRepository.cs*. Actualmente en que método tiene una consulta de objeto que tiene `Include` métodos para la `Person` y `Courses` propiedades de navegación. Reemplace el `return` instrucción con el código que realiza la carga explícita, como se muestra en el ejemplo siguiente:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample14.cs)]

Ejecute el *Departments.aspx* página en el depurador y comprobar la **IntelliTrace** ventana nuevo como se hacía anteriormente. Ahora, donde había una sola consulta antes, verá una secuencia larga de ellos.

[![Image13](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image22.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image21.png)

Haga clic en la primera **ADO.NET** línea para ver lo que ha sucedido en la consulta compleja ve versiones anteriores.

[![Image14](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image24.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image23.png)

La consulta de departamentos se ha convertido en un sencillo `Select` de consulta y no tienen ninguna `Join` cláusula, pero va seguido de separar las consultas que recuperan cursos relacionados y un administrador, mediante un conjunto de dos consultas para cada departamento devuelto por la versión original consulta.

> [!NOTE]
> Si deja diferida carga habilitada, el patrón que ve aquí, con la misma consulta repetirse muchas veces, podría deberse a carga diferida. Un patrón que probablemente le interese evitar es carga diferida datos relacionados para cada fila de la tabla principal. A menos que haya comprobado que una consulta de combinación única es demasiado compleja para ser eficaz, normalmente podrá mejorar el rendimiento en casos como este, cambie la consulta principal para utilizar carga diligente.


## <a name="pre-generating-views"></a>La generación de vistas previas

Cuando un `ObjectContext` primero se crea el objeto en un nuevo dominio de aplicación, Entity Framework genera un conjunto de clases que utiliza para tener acceso a la base de datos. Estas clases se denominan *vistas*, y si tiene un modelo de datos muy grandes, generar estas vistas puede retrasar la respuesta del sitio web a la primera solicitud para una página después de que se inicializa un nuevo dominio de aplicación. Puede reducir este retraso de la primera solicitud mediante la creación de las vistas en tiempo de compilación en lugar de en tiempo de ejecución.

> [!NOTE]
> Si la aplicación no tiene un modelo de datos extremadamente grandes, o si tiene un modelo de datos de gran tamaño pero no le preocupa un problema de rendimiento que afecta a solo la primera solicitud de página después de que IIS se recicla, puede omitir esta sección. Vista de creación no se presenta cada vez que cree instancias de un `ObjectContext` de objeto, dado que las vistas se almacenan en caché en el dominio de aplicación. Por lo tanto, a menos que se está con frecuencia, el reciclaje de la aplicación en IIS muy pocas solicitudes de página se beneficiarían de vistas generadas previamente.


Se pueden generar previamente las vistas mediante el *EdmGen.exe* herramienta de línea de comandos o mediante un *Kit de herramientas de transformación de plantillas de texto* plantilla (T4). En este tutorial usará una plantilla T4.

En el *DAL* carpeta, agregue un archivo mediante la **plantilla de texto** plantilla (está bajo la **General** nodo en el **plantillas instaladas** lista), y asígnele el nombre *SchoolModel.Views.tt*. Reemplace el código existente en el archivo con el código siguiente:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample15.cs)]

Este código genera vistas para una *.edmx* archivo que se encuentra en la misma carpeta que la plantilla y que tiene el mismo nombre que el archivo de plantilla. Por ejemplo, si su archivo de plantilla se denomina *SchoolModel.Views.tt*, buscará un archivo de modelo de datos denominado *SchoolModel.edmx*.

Guarde el archivo, a continuación, haga clic en el archivo en **el Explorador de soluciones** y seleccione **ejecutar herramienta personalizada**.

[![Image02](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image26.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image25.png)

Visual Studio genera un archivo de código que crea las vistas, que se denomina *SchoolModel.Views.cs* basado en la plantilla. (Es posible que haya observado que el archivo de código se genera antes de seleccionar **ejecutar herramienta personalizada**, tan pronto como se guarda el archivo de plantilla.)

[![Image01](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image28.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image27.png)

Ahora puede ejecutar la aplicación y comprobar que funciona igual que antes.

Para obtener más información acerca de las vistas generadas previamente, vea los siguientes recursos:

- [Cómo: generar previamente las vistas para mejorar el rendimiento de las consultas](https://msdn.microsoft.com/library/bb896240.aspx) en el sitio web MSDN. Explica cómo utilizar el `EdmGen.exe` herramienta de línea de comandos para generar previamente las vistas.
- [Aislar el rendimiento con precompilado/previa-generated vistas en Entity Framework 4](https://blogs.msdn.com/b/appfabriccat/archive/2010/08/06/isolating-performance-with-precompiled-pre-generated-views-in-the-entity-framework-4.aspx) en el blog del equipo de asesoramiento de cliente de Windows Server AppFabric.

Con esto finaliza la introducción a mejorar el rendimiento en una aplicación web ASP.NET que utiliza Entity Framework. Para obtener más información, vea los siguientes recursos:

- [Consideraciones de rendimiento (Entity Framework)](https://msdn.microsoft.com/library/cc853327.aspx) en el sitio web MSDN.
- [Entradas relacionadas con el rendimiento en el blog del equipo de Entity Framework](https://blogs.msdn.com/b/adonet/archive/tags/performance/).
- [EF opciones de combinación y consultas compiladas](https://blogs.msdn.com/b/dsimmons/archive/2010/01/12/ef-merge-options-and-compiled-queries.aspx). Entrada de blog que explica comportamientos inesperados de consultas compiladas y mezcla opciones tales como `NoTracking`. Si planea usar consultas compiladas o manipular la configuración de la opción de combinación en la aplicación, lea esto primero.
- [Entidad relacionada con el marco de trabajo se registra en el blog del equipo de asesoramiento al cliente de modelado y datos](https://blogs.msdn.com/b/dmcat/archive/tags/entity+framework/). Incluye entradas sobre las consultas compiladas y utilizar el generador de perfiles de Visual Studio 2010 para detectar problemas de rendimiento.
- [Subproceso del foro de Entity Framework con consejos sobre cómo mejorar el rendimiento de las consultas muy complejas](https://social.msdn.microsoft.com/Forums/adodotnetentityframework/thread/ffe8b2ab-c5b5-4331-8988-33a872d0b5f6).
- [Recomendaciones de administración de estado de ASP.NET](https://msdn.microsoft.com/library/z1hkazw7.aspx).
- [Con Entity Framework y ObjectDataSource: paginación personalizada](http://geekswithblogs.net/Frez/articles/using-the-entity-framework-and-the-objectdatasource-custom-paging.aspx). Entrada de blog que se basa en la aplicación de ContosoUniversity creada en estos tutoriales para explicar cómo implementar la paginación en el *Departments.aspx* página.

El siguiente tutorial revisa algunas de las mejoras importantes a Entity Framework que son nuevas en la versión 4.

> [!div class="step-by-step"]
> [Anterior](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md)
> [Siguiente](what-s-new-in-the-entity-framework-4.md)
