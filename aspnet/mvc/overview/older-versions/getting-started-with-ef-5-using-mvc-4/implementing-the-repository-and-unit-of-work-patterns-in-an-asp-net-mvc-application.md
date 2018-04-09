---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application
title: Implementar el repositorio y una unidad de patrones de trabajo en una aplicación de MVC de ASP.NET (9 de 10) | Documentos de Microsoft
author: tdykstra
description: La aplicación web de ejemplo de la Universidad de Contoso muestra cómo crear aplicaciones de ASP.NET MVC 4 mediante Code First de Entity Framework 5 y Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 44761193-04ba-4990-9f90-145d3c10a716
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 1f870b61658686769304a7809bde62e66da3bd0c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="implementing-the-repository-and-unit-of-work-patterns-in-an-aspnet-mvc-application-9-of-10"></a>Implementar el repositorio y una unidad de patrones de trabajo en una aplicación de MVC de ASP.NET (9 de 10)
====================
por [Tom Dykstra](https://github.com/tdykstra)

[Descargar el proyecto completado](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> La aplicación web de ejemplo de la Universidad de Contoso muestra cómo crear aplicaciones de ASP.NET MVC 4 con Code First de Entity Framework 5 y Visual Studio 2012. Para obtener información sobre la serie de tutoriales, consulte [el primer tutorial de la serie](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Puede iniciar la serie de tutoriales desde el principio o [descargar un proyecto de inicio para este capítulo](building-the-ef5-mvc4-chapter-downloads.md) y empiece aquí.
> 
> > [!NOTE] 
> > 
> > Si experimenta un problema no se puede resolver, [descargar el capítulo completado](building-the-ef5-mvc4-chapter-downloads.md) e intente reproducir el problema. Por lo general puede encontrar la solución al problema comparando el código para el código completo. Para que algunos errores comunes y cómo resolverlos, vea [errores y soluciones alternativas.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


En el tutorial anterior usa la herencia para reducir el código redundante en la `Student` y `Instructor` las clases de entidad. En este tutorial, verá algunas formas de usar el repositorio y una unidad de patrones de trabajo para las operaciones CRUD. Como se muestra en el tutorial anterior, en éste va a cambiar la manera en que el código funciona con páginas que ya creado, en lugar de crear nuevas páginas.

## <a name="the-repository-and-unit-of-work-patterns"></a>El repositorio y una unidad de patrones de trabajo

El repositorio y una unidad de patrones de trabajo están diseñados para crear una capa de abstracción entre la capa de acceso a datos y la capa de lógica empresarial de una aplicación. Implementar estos patrones puede ayudar a aislar la aplicación de cambios en el almacén de datos y puede facilitar la realización de pruebas unitarias automatizadas o el desarrollo controlado por pruebas (TDD).

En este tutorial se implementa una clase de repositorio para cada tipo de entidad. Para el `Student` tipo de entidad que se creará una interfaz del repositorio y una clase de repositorio. Cuando se crean instancias del repositorio en el controlador, deberá usar la interfaz para que el controlador acepta una referencia a cualquier objeto que implementa la interfaz del repositorio. Cuando el controlador se ejecuta en un servidor web, recibe un repositorio que funciona con Entity Framework. Cuando el controlador se ejecuta en una clase de prueba unitaria, recibe un repositorio que funciona con los datos almacenados de forma que se puede manipular fácilmente para las pruebas, como una colección en memoria.

Más adelante en el tutorial usará varias bases de datos y una unidad de la clase de trabajo para el `Course` y `Department` tipos de entidad en el `Course` controlador. La unidad de trabajo clase coordina el trabajo de varias bases de datos mediante la creación de una clase de contexto de base de datos única compartida por todas ellas. Si desea poder realizar pruebas unitarias automatizadas, podría crear y usar interfaces para estas clases de la misma manera que hizo el `Student` repositorio. Sin embargo, para simplificar el tutorial, podrá crear y usar estas clases sin interfaces.

En la siguiente ilustración muestra una manera conceptualizar las relaciones entre las clases de contexto en comparación con no utiliza el repositorio o la unidad de trabajo patrón en absoluto y el controlador.

![Repository_pattern_diagram](https://asp.net/media/2578149/Windows-Live-Writer_8c4963ba1fa3_CE3B_Repository_pattern_diagram_1df790d3-bdf2-4c11-9098-946ddd9cd884.png)

No crear pruebas unitarias en esta serie de tutoriales. Para obtener una introducción sobre TDD con una aplicación de MVC que usa el modelo de repositorio, consulte [Tutorial: utilizar TDD con ASP.NET MVC](https://msdn.microsoft.com/library/ff847525.aspx). Para obtener más información sobre el modelo de repositorio, consulte los siguientes recursos:

- [El modelo de repositorio](https://msdn.microsoft.com/library/ff649690.aspx) en MSDN.
- [Uso de patrones de repositorio y unidad de trabajo con Entity Framework 4.0](https://blogs.msdn.com/b/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx) en el blog del equipo de Entity Framework.
- [Repositorio de Agile Entity Framework 4](http://thedatafarm.com/blog/data-access/agile-entity-framework-4-repository-part-1-model-and-poco-classes/) serie de entradas de blog de Julie Lerman.
- [Creación de la cuenta en una aplicación de HTML5/jQuery vista](https://weblogs.asp.net/dwahlin/archive/2011/08/15/building-the-account-at-a-glance-html5-jquery-application.aspx) en el blog de Wahlin.

> [!NOTE]
> Hay muchas maneras de implementar el repositorio y una unidad de patrones de trabajo. Puede usar las clases de repositorio con o sin una unidad de la clase de trabajo. Puede implementar un único repositorio para todos los tipos de entidad, o uno para cada tipo. Si implementa una para cada tipo, puede usar clases independientes, una clase base genérica y las clases derivadas, o una clase base abstracta y las clases derivadas. Puede incluir lógica de negocios en el repositorio o restringirlo a la lógica de acceso a datos. También puede crear una capa de abstracción en la clase de contexto de base de datos mediante el uso de [IDbSet](https://msdn.microsoft.com/library/gg679233(v=vs.103).aspx) hay interfaces en lugar de [DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=vs.103).aspx) tipos para los conjuntos de entidades. El enfoque para implementar una capa de abstracción que se muestra en este tutorial es una opción para tener en cuenta, no una recomendación para todos los escenarios y entornos.


## <a name="creating-the-student-repository-class"></a>Crear la clase de repositorio de estudiante

En el *DAL* carpeta, cree un archivo de clase denominado *IStudentRepository.cs* y reemplace el código existente por el código siguiente:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample1.cs)]

Este código declara un conjunto típico de métodos CRUD, incluidos los dos métodos de lectura, que todos los devuelve `Student` entidades y otro que busca una sola `Student` entidad por identificador.

En el *DAL* carpeta, cree un archivo de clase denominado *StudentRepository.cs* archivo. Reemplace el código existente por el código siguiente, que implementa el `IStudentRepository` interfaz:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample2.cs)]

El contexto de base de datos se define en una variable de clase y el constructor espera que el objeto que realiza la llamada debe pasar en una instancia del contexto:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample3.cs)]

Puede crear una instancia de un nuevo contexto en el repositorio, pero, a continuación, si utiliza varias bases de datos en un controlador, cada uno de ellos podría terminar con un contexto independiente. Más adelante podrá utilizar varias bases de datos en el `Course` controlador y verá cómo puede asegurarse de que todos los repositorios utilizan el mismo contexto de una unidad de la clase de trabajo.

Implementa el repositorio [IDisposable](https://msdn.microsoft.com/library/system.idisposable.aspx) y desecha el contexto de base de datos tal y como se vio anteriormente en el controlador y sus métodos CRUD realizan llamadas en el contexto de base de datos de la misma manera que vio anteriormente.

## <a name="change-the-student-controller-to-use-the-repository"></a>Cambiar el controlador de estudiante para utilizar el repositorio

En *StudentController.cs*, reemplace el código actualmente en la clase con el código siguiente. Los cambios aparecen resaltados.

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=13-18,44,75,77,102-103,120,137-138,159,172-174,186)]

El controlador ahora declara una variable de clase para un objeto que implementa el `IStudentRepository` interfaz en lugar de la clase de contexto:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample5.cs)]

El constructor predeterminado (sin parámetros) crea una nueva instancia de contexto y un constructor opcional permite que el llamador pase en una instancia de contexto.

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample6.cs)]

(Si estaba usando *inyección de dependencia*, o la inserción de dependencias, no sería necesario el constructor predeterminado porque el software DI aseguraría que el objeto de repositorio correcto siempre proporcionaría.)

En los métodos CRUD, ahora se denomina el repositorio en lugar del contexto:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample7.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample8.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample9.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample10.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample11.cs)]

Y `Dispose` método desecha ahora el repositorio en lugar del contexto:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample12.cs)]

Ejecute el sitio Web y haga clic en el **estudiantes** ficha.

![Students_Index_page](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image1.png)

La página de consulta y funciona igual que lo hacía antes de cambiar el código para utilizar el repositorio y las otras páginas de estudiante también funcionan igual. Sin embargo, hay una diferencia importante en la forma del `Index` hace el método del controlador de filtrado y ordenación. La versión original de este método contiene el código siguiente:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample13.cs?highlight=1)]

La actualización `Index` método contiene el código siguiente:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample14.cs?highlight=1)]

Solo el código resaltado ha cambiado.

En la versión original del código, `students` se escribe como un `IQueryable` objeto. La consulta no se envía a la base de datos hasta que se convierte en una colección utilizando un método como `ToList`, que no se produce hasta que la vista de índice tiene acceso al modelo de estudiante. El `Where` método en el código original anterior se convierte en un `WHERE` cláusula de la consulta SQL que se envía a la base de datos. A su vez, esto significa que solo las entidades seleccionadas se devuelven por la base de datos. Sin embargo, como resultado de cambiar `context.Students` a `studentRepository.GetStudents()`, `students` variable después de esta instrucción es un `IEnumerable` colección que incluye todos los alumnos en la base de datos. El resultado final de aplicar el `Where` método es el mismo, pero ahora se realiza el trabajo en memoria en el servidor web y no por la base de datos. Para las consultas que devuelven grandes volúmenes de datos, esto puede ser ineficaz.

> [!TIP]
> 
> **Vs IQueryable. IEnumerable**
> 
> Después de implementar el repositorio, como se muestra aquí, incluso si escribe algo en la **búsqueda** cuadro de la consulta enviada al servidor de SQL devuelve todas las filas de estudiante porque no incluye los criterios de búsqueda:
> 
> ![](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image2.png)
> 
> [!code-sql[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample15.sql)]
> 
> Esta consulta devuelve todos los datos de estudiante, dado que el repositorio ejecuta la consulta sin conocer acerca de los criterios de búsqueda. El proceso de ordenación, aplicar los criterios de búsqueda y seleccionar un subconjunto de los datos de paginación (que se muestran solo 3 filas en este caso) se realiza en la memoria cuando más adelante el `ToPagedList` método se llama en el `IEnumerable` colección.
> 
> En la versión anterior del código (antes de implementar el repositorio), la consulta no se envía a la base de datos hasta después de aplicar los criterios de búsqueda, cuando `ToPagedList` se llama en el `IQueryable` objeto.
> 
> ![](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image3.png)
> 
> Cuando se llama a ToPagedList en un `IQueryable` de objetos, la consulta enviada al servidor de SQL especifica la cadena de búsqueda y así se devuelven únicamente las filas que cumplen los criterios de búsqueda y ningún filtrado debe hacerse en la memoria.
> 
> [!code-sql[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample16.sql)]
> 
> (El tutorial siguiente explica cómo examinar las consultas enviadas a SQL Server).


La sección siguiente muestra cómo implementar los métodos de repositorio que permiten especificar que este trabajo debe hacerse la base de datos.

Ahora ha creado una capa de abstracción entre el controlador y el contexto de base de datos de Entity Framework. Si se va a realizar pruebas con esta aplicación unitarias automatizadas, podría crear una clase de repositorio alternativo en un proyecto de prueba unitaria que implementa `IStudentRepository` *.* En lugar de llamar el contexto para leer y escribir datos, esta clase de repositorio ficticio podría manipular colecciones en memoria para probar las funciones del controlador.

## <a name="implement-a-generic-repository-and-a-unit-of-work-class"></a>Implementar un repositorio genérico y una unidad de trabajo (clase)

Crear una clase de repositorio para cada tipo de entidad podría dar lugar a una gran cantidad de código redundante y podría dar lugar a actualizaciones parciales. Por ejemplo, suponga que tiene que actualizar dos tipos de entidad diferente como parte de la misma transacción. Si cada uno de ellos usa una instancia de contexto de base de datos independientes, uno podría realizarse correctamente y la otra podría producir un error. Una manera de minimizar el código redundante es usar un repositorio genérico y una manera de asegurarse de que todos los repositorios utilizan el mismo contexto de base de datos (y, por tanto, coordinan todas las actualizaciones) consiste en utilizar una unidad de la clase de trabajo.

En esta sección del tutorial, creará un `GenericRepository` clase y un `UnitOfWork` clase y usarlos en el `Course` controlador para tener acceso el `Department` y `Course` conjuntos de entidades. Como se explicó anteriormente, para simplificar esta parte del tutorial, no es crear interfaces para estas clases. Pero si se va a utilizarlos para facilitar TDD, normalmente debería implementarlos con interfaces de la misma manera que el `Student` repositorio.

### <a name="create-a-generic-repository"></a>Crear un repositorio genérico

En el *DAL* carpeta, crear *GenericRepository.cs* y reemplace el código existente por el código siguiente:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample17.cs)]

Se declaran variables de clase para el contexto de base de datos y el conjunto de entidades que se crea una instancia en el repositorio para:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample18.cs)]

El constructor acepta una instancia de contexto de base de datos y la inicializa la variable de conjunto de entidades:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample19.cs)]

El `Get` método utiliza expresiones lambda para permitir que el código que realiza la llamada especificar una condición de filtro y una columna para ordenar los resultados por y un parámetro de cadena permite que el llamador proporcionar una lista delimitada por comas de las propiedades de navegación para carga diligente:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample20.cs)]

El código `Expression<Func<TEntity, bool>> filter` significa que el autor de llamada proporcionará una expresión lambda tomando como base el `TEntity` tipo y esta expresión devolverá un valor booleano. Por ejemplo, si el repositorio se crea una instancia de la `Student` tipo de entidad, puede especificar el código del método de llamada `student => student.LastName == "Smith` &quot; para el `filter` parámetro.

El código `Func<IQueryable<TEntity>, IOrderedQueryable<TEntity>> orderBy` también significa que el autor de llamada proporcionará una expresión lambda. Pero en este caso, la entrada a la expresión es un `IQueryable` de objeto para el `TEntity` tipo. La expresión devolverá una versión ordenada de ese `IQueryable` objeto. Por ejemplo, si el repositorio se crea una instancia de la `Student` tipo de entidad, puede especificar el código del método de llamada `q => q.OrderBy(s => s.LastName)` para el `orderBy` parámetro.

El código en el `Get` método crea un `IQueryable` de objeto y, a continuación, se aplica la expresión de filtro, si hay alguno:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample21.cs)]

A continuación se aplica a las expresiones de carga diligente después del análisis de la lista delimitada por comas:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample22.cs)]

Por último, se aplica el `orderBy` expresión si lo hay y devuelve los resultados; en caso contrario, devuelve los resultados de la consulta sin ordenar:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample23.cs)]

Cuando se llama a la `Get` método, se pudieron hacer de filtrado y ordenación en la `IEnumerable` colección devuelta por el método en lugar de proporcionar parámetros para estas funciones. Pero la ordenación y filtrado de trabajo, a continuación, se haría en la memoria en el servidor web. Mediante el uso de estos parámetros, se asegura de que el trabajo se realiza la base de datos en lugar de con el servidor web. Una alternativa consiste en crear las clases derivadas para tipos de entidad concreta y agregar especializada `Get` métodos, como `GetStudentsInNameOrder` o `GetStudentsByName`. Sin embargo, en una aplicación compleja, esto puede dar lugar a un gran número de estas clases derivadas y métodos especializados, lo que sería más trabajo para mantener.

El código en el `GetByID`, `Insert`, y `Update` métodos es similar a lo que ha visto en el repositorio no genérica. (No se proporciona un parámetro de carga diligente de la `GetByID` firma, porque no puede realizar una carga diligente con el `Find` método.)

Se proporcionan dos sobrecargas para el `Delete` método:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample24.cs)]

Uno de estos permite pasar en tan solo el identificador de la entidad que se eliminará y una toma una instancia de entidad. Como vimos en el [control de simultaneidad](../../getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md) tutorial para la simultaneidad de control necesita un `Delete` método que toma una instancia de entidad que incluye el valor original de una propiedad de seguimiento.

Este repositorio genérico controlará requisitos CRUD típicos. Cuando un tipo de entidad concreto tiene requisitos especiales, como el filtrado más complejas o la ordenación, puede crear una clase derivada que tiene métodos adicionales para ese tipo.

## <a name="creating-the-unit-of-work-class"></a>Crear la unidad de trabajo (clase)

La unidad de la clase de trabajo tiene una finalidad: para asegurarse de que, cuando utilice varios repositorios, comparten un contexto de base de datos único. De este modo, una vez completada una unidad de trabajo se puede llamar a la `SaveChanges` método en esa instancia del contexto y estar seguros todos relacionados con los cambios serán coordinadas. Todo lo que las necesidades de la clase es un `Save` método y una propiedad de cada repositorio. Cada propiedad de repositorio devuelve una instancia de repositorio que se ha creado una instancia con la misma instancia de contexto de base de datos como las demás instancias de repositorio.

En el *DAL* carpeta, cree un archivo de clase denominado *UnitOfWork.cs* y reemplace el código de plantilla con el código siguiente:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample25.cs)]

El código crea las variables de clase para el contexto de base de datos y cada repositorio. Para el `context` variable, se crea un nuevo contexto:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample26.cs)]

Cada propiedad de repositorio comprueba si el repositorio ya existe. De lo contrario, crea una instancia del repositorio, pasando la instancia de contexto. Como resultado, todos los repositorios comparten la misma instancia de contexto.

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample27.cs)]

El `Save` llamadas al método `SaveChanges` en el contexto de base de datos.

Como con cualquier clase que crea una instancia de un contexto de base de datos en una variable de clase, el `UnitOfWork` la clase implementa `IDisposable` y desecha el contexto.

### <a name="changing-the-course-controller-to-use-the-unitofwork-class-and-repositories"></a>Cambiar el controlador de curso para usar la clase UnitOfWork y repositorios

Reemplace el código que tiene actualmente en *CourseController.cs* con el código siguiente:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample28.cs?highlight=15,20,22,31,54-55,70,85-86,101-102,122-124,130)]

Este código agrega una variable de clase para la `UnitOfWork` clase. (Si estaba usando interfaces aquí, no sería inicializar aquí, la variable; en su lugar, debería implementar un patrón de los dos constructores como hizo el `Student` repositorio.)

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample29.cs)]

En el resto de la clase, todas las referencias en el contexto de base de datos se ha reemplazado por las referencias en el repositorio adecuado, con `UnitOfWork` propiedades para tener acceso al repositorio. El `Dispose` método elimina el `UnitOfWork` instancia.

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample30.cs)]

Ejecute el sitio Web y haga clic en el **cursos** ficha.

![Courses_Index_page](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image4.png)

La página de consulta y funciona igual a como lo hacía antes de los cambios y las otras páginas de curso también funcionan igual.

## <a name="summary"></a>Resumen

Ahora ha implementado el repositorio y la unidad de patrones de trabajo. Ha usado las expresiones lambda como parámetros de método en el repositorio genérico. Para obtener más información sobre cómo usar estas expresiones con un `IQueryable` de objetos, consulte [IQueryable(T) interfaz (System.Linq)](https://msdn.microsoft.com/library/bb351562.aspx) en MSDN Library. En el siguiente tutorial, obtendrá información sobre cómo controlar algunos escenarios avanzados.

Vínculos a otros recursos de Entity Framework pueden encontrarse en el [mapa de contenido de acceso de datos de ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Anterior](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [Siguiente](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)
