---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
title: Implementar la herencia con Entity Framework en una aplicación ASP.NET MVC (8 de 10) | Microsoft Docs
author: tdykstra
description: La aplicación web de Contoso University muestra cómo crear aplicaciones de ASP.NET MVC 4 mediante Code First de Entity Framework 5 y Visual Studio...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: a5c3feff-5335-4cdd-a97d-f7a8785c2494
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 10ee0be62a1d601e323afc423e9022bed56f4f33
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837535"
---
<a name="implementing-inheritance-with-the-entity-framework-in-an-aspnet-mvc-application-8-of-10"></a>Implementar la herencia con Entity Framework en una aplicación ASP.NET MVC (8 de 10)
====================
por [Tom Dykstra](https://github.com/tdykstra)

[Descargue el proyecto completado](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> La aplicación web de Contoso University muestra cómo crear aplicaciones de ASP.NET MVC 4 mediante Code First de Entity Framework 5 y Visual Studio 2012. Para obtener información sobre la serie de tutoriales, consulte [el primer tutorial de la serie](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Puede iniciar la serie de tutoriales desde el principio o [descargar un proyecto de inicio de este capítulo](building-the-ef5-mvc4-chapter-downloads.md) y comience aquí.
> 
> > [!NOTE] 
> > 
> > Si surge un problema que no se puede resolver, [descargar el capítulo completado](building-the-ef5-mvc4-chapter-downloads.md) e intente reproducir el problema. Por lo general puede encontrar la solución al problema comparando el código para el código completo. Para que algunos errores comunes y cómo resolverlos, consulte [errores y soluciones alternativas.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


En el tutorial anterior controla las excepciones de simultaneidad. En este tutorial se muestra cómo implementar la herencia en el modelo de datos.

En la programación orientada a objetos, puede usar la herencia para eliminar código redundante. En este tutorial, cambiará las clases `Instructor` y `Student` para que deriven de una clase base `Person` que contenga propiedades como `LastName`, que son comunes tanto para los instructores como para los alumnos. No tendrá que agregar ni cambiar ninguna página web, sino que cambiará parte del código y esos cambios se reflejarán automáticamente en la base de datos.

## <a name="table-per-hierarchy-versus-table-per-type-inheritance"></a>Tabla por jerarquía en comparación con la herencia de tabla por tipo

En la programación orientada a objetos, puede usar la herencia para que sea más fácil trabajar con las clases relacionadas. Por ejemplo, el `Instructor` y `Student` clases en el `School` modelo de datos comparten varias propiedades, que da como resultado código redundante:

![Student_and_Instructor_classes](https://asp.net/media/2578113/Windows-Live-Writer_58f5a93579b2_CC7B_Student_and_Instructor_classes_e7a32f99-8bc4-48ce-aeaf-216a18071a8b.png)

Imagine que quiere eliminar el código redundante de las propiedades que comparten las entidades `Instructor` y `Student`. Puede crear un `Person` clase base que contiene solo las propiedades compartidas y, después, realizar la `Instructor` y `Student` entidades hereden de esa clase base, como se muestra en la ilustración siguiente:

![Student_and_Instructor_classes_deriving_from_Person_class](https://asp.net/media/2578119/Windows-Live-Writer_58f5a93579b2_CC7B_Student_and_Instructor_classes_deriving_from_Person_class_671d708c-cbb8-454a-a8f8-c2d99439acd9.png)

Esta estructura de herencia se puede representar de varias formas en la base de datos. Podría tener un `Person` tabla que incluye información acerca de los estudiantes y profesores en una sola tabla. Algunas de las columnas podrían aplicarse solo a los instructores (`HireDate`), otras solo a los alumnos (`EnrollmentDate`), algunos en ambas (`LastName`, `FirstName`). Por lo general, tendría un *discriminador* columna para indicar qué tipo de cada fila representa. Por ejemplo, en la columna discriminadora podría aparecer "Instructor" para los instructores y "Student" para los alumnos.

![Tabla por hierarchy_example](https://asp.net/media/2578125/Windows-Live-Writer_58f5a93579b2_CC7B_Table-per-hierarchy_example_244067cd-b451-4e9b-9595-793b9afca505.png)

Este patrón de generar una estructura de herencia de entidad de una tabla de base de datos única se denomina *tabla por jerarquía* herencia (TPH).

Una alternativa consiste en hacer que la base de datos se parezca más a la estructura de herencia. Por ejemplo, podría tener solo los campos de nombre el `Person` de tabla y tenga distintos `Instructor` y `Student` tablas con los campos de fecha.

![Tabla por type_inheritance](https://asp.net/media/2578131/Windows-Live-Writer_58f5a93579b2_CC7B_Table-per-type_inheritance.png)

Este patrón de hacer que una tabla de base de datos para cada clase de entidad se denomina *tabla por tipo* herencia (TPT).

Patrones de herencia de TPH acostumbran un mejor rendimiento en Entity Framework que patrones de herencia de TPT, porque estos pueden provocar consultas de combinaciones complejas. Este tutorial muestra cómo implementar la herencia de TPH. Hágalo mediante los pasos siguientes:

- Crear un `Person` clase y cambie el `Instructor` y `Student` clases se deriven de `Person`.
- Agregue el código de asignación de modelo para bases de datos a la clase de contexto de base de datos.
- Cambio `InstructorID` y `StudentID` referencias a lo largo del proyecto a `PersonID`.

## <a name="creating-the-person-class"></a>Creación de la clase Person

 Nota: No podrá compilar el proyecto después de crear las clases siguientes hasta que actualice los controladores que usa estas clases. 

En el *modelos* carpeta, cree *Person.cs* y reemplace el código de plantilla con el código siguiente:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

En *Instructor.cs*, derivar el `Instructor` clase desde el `Person` clase y quite los campos claves y nombres. El código tendrá un aspecto similar al ejemplo siguiente:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Realizar modificaciones similares a *Student.cs*. La `Student` clase tendrá un aspecto similar al ejemplo siguiente:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

## <a name="adding-the-person-entity-type-to-the-model"></a>Agregar el tipo de entidad Person al modelo

En *SchoolContext.cs*, agregue un `DbSet` propiedad para el `Person` tipo de entidad:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

Esto es todo lo que Entity Framework necesita para configurar la herencia de tabla por jerarquía. Como verá, cuando se vuelve a crear, la base de datos tendrá un `Person` de tabla en lugar de la `Student` y `Instructor` tablas.

## <a name="changing-instructorid-and-studentid-to-personid"></a>Cambiar InstructorID y StudentID a PersonID

En *SchoolContext.cs*, en la instrucción de asignación de Instructor cursos, cambiar `MapRightKey("InstructorID")` a `MapRightKey("PersonID")`:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=4)]

Este cambio no es necesario; solo cambia el nombre de la columna InstructorID en la tabla de combinación de varios a varios. Si se deja el nombre como InstructorID, la aplicación aún funcionaría correctamente. Aquí está completado *SchoolContext.cs*:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs?highlight=15,24)]

A continuación debe cambiar `InstructorID` a `PersonID` y `StudentID` a `PersonID` en todo el proyecto ***excepto*** en los archivos de migraciones con marca de tiempo en el *migraciones* carpeta. Hacer encontrar y abrir sólo los archivos que deben cambiarse y realizar un cambio global en los archivos abiertos. El único archivo en el *migraciones* debe cambiar la carpeta es *migrations\configuration. cs.*

1. > [!IMPORTANT]
   > Comience por cerrar todos los archivos abiertos en Visual Studio.
2. Haga clic en **buscar y reemplazar: encontrar todos los archivos** en el **editar** menú y, a continuación, buscar todos los archivos del proyecto que contienen `InstructorID`.  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)
3. Abra cada archivo en el **resultados de la búsqueda** ventana ***excepto*** el &lt;marca de tiempo&gt;*\_.cs* los archivos de migración en el *Migraciones* carpeta haciendo doble clic en una línea para cada archivo.  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)
4. Abra el **reemplazar en archivos** cuadro de diálogo y cambio **buscar en** a **todos los documentos abiertos**.
5. Use la **reemplazar en archivos** cuadro de diálogo para cambiar todas `InstructorID` a `PersonID.`  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)
6. Buscar todos los archivos en el proyecto que contienen `StudentID`.
7. Abra cada archivo en el **resultados de la búsqueda** ventana ***excepto*** el &lt;marca de tiempo&gt;*\_\*.cs* los archivos de migración en el *migraciones* carpeta haciendo doble clic en una línea para cada archivo.  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)
8. Abra el **reemplazar en archivos** cuadro de diálogo y cambio **buscar en** a **todos los documentos abiertos**.
9. Use la **reemplazar en archivos** cuadro de diálogo para cambiar todas `StudentID` a `PersonID`.   
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)
10. Compile el proyecto.

(Tenga en cuenta que esto demuestra un *desventaja* de la `classnameID` patrón para asignar nombres a las claves principales. Si se hubiera designado claves principales identificador sin el nombre de la clase *ningún* cambiar el nombre sería necesario ahora.)

## <a name="create-and-update-a-migrations-file"></a>Crear y actualizar un archivo de migraciones

En la consola de administrador de paquetes (PMC), escriba el siguiente comando:

`Add-Migration Inheritance`

Ejecute el `Update-Database` comando en la PMC. El comando fallará en este momento porque tenemos las migraciones no sabe cómo tratar los datos existentes. Muestra el siguiente error:

*La instrucción ALTER TABLE en conflicto con la restricción FOREIGN KEY "FK\_dbo. Departamento\_dbo. Persona\_PersonID ". Se produjo el conflicto en la tabla base de datos "contosouniversity" por "dbo. Persona", columna 'PersonID'.*

Abra *migraciones\&lt; marca de tiempo&gt;\_Inheritance.cs* y reemplace el `Up` método con el código siguiente:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=25,29-40)]

Ejecute el `update-database` nuevo comando.

> [!NOTE]
> Es posible generar otros errores al migrar datos y realizar cambios de esquema. Si se producen errores de migración no puede resolver, puede continuar con el tutorial, cambie la cadena de conexión en el *Web.config* archivo o la eliminación de la base de datos. El enfoque más sencillo consiste en cambiar el nombre de la base de datos en el *Web.config* archivo. Por ejemplo, cambiar el nombre de la base de datos a CU\_probar tal y como se muestra en el ejemplo siguiente:
> 
> [!code-xml[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.xml?highlight=1-2)]
> 
> Con una base de datos, no hay ningún dato para migrar y el `update-database` comando es mucho más probable que se completará sin errores. Para obtener instrucciones sobre cómo eliminar la base de datos, consulte [cómo quitar una base de datos de Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/). Si adopta este enfoque para poder continuar con el tutorial, omita el paso de implementación al final de este tutorial, dado que el sitio implementado obtendría el mismo error cuando se ejecutan automáticamente las migraciones. Si desea solucionar un error de las migraciones, el mejor recurso es uno de los foros de Entity Framework o StackOverflow.com.


## <a name="testing"></a>Pruebas

Ejecute el sitio Web y prueba en distintas páginas. Todo funciona igual que antes.

En **Explorador de servidores,** expanda **SchoolContext** y, a continuación, **tablas**, y verá que el **estudiante** y **Instructor**  tablas han sido reemplazadas por un **persona** tabla. Expanda el **persona** tabla y verá que tiene todas las columnas que solían haber en el **estudiante** y **Instructor** tablas.

![Server_Explorer_showing_Person_table](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Haga clic con el botón derecho en la tabla Person y después haga clic en **Mostrar datos de tabla** para ver la columna discriminadora.

![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

El siguiente diagrama muestra la estructura de la nueva base de datos School:

![School_database_diagram](https://asp.net/media/2578143/Windows-Live-Writer_58f5a93579b2_CC7B_School_database_diagram_6350a801-7199-413f-bbac-4a2009ed19d7.png)

## <a name="summary"></a>Resumen

Herencia de tabla por jerarquía ahora se ha implementado para el `Person`, `Student`, y `Instructor` clases. Para obtener más información acerca de ésta y otras estructuras de herencia, vea [estrategias de asignación de herencia](https://weblogs.asp.net/manavi/archive/2010/12/24/inheritance-mapping-strategies-with-entity-framework-code-first-ctp5-part-1-table-per-hierarchy-tph.aspx) en el blog de Morteza Manavi. En el siguiente tutorial verá algunas maneras de implementar el repositorio y unidad de patrones de trabajo.

Pueden encontrar vínculos a otros recursos de Entity Framework en el [mapa de contenido de acceso de datos de ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Anterior](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [Siguiente](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md)
