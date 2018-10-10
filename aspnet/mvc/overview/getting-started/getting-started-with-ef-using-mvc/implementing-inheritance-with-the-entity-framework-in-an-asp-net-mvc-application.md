---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
title: Implementar la herencia con Entity Framework 6 en una aplicación de ASP.NET MVC 5 (11 de 12) | Microsoft Docs
author: tdykstra
description: La aplicación web de Contoso University muestra cómo crear aplicaciones de ASP.NET MVC 5 con Entity Framework 6 Code First y Visual Studio...
ms.author: riande
ms.date: 11/07/2014
ms.assetid: 08834147-77ec-454a-bb7a-d931d2a40dab
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 613494d58d7652f69a52241bcd3a7e896bc5407c
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912714"
---
<a name="implementing-inheritance-with-the-entity-framework-6-in-an-aspnet-mvc-5-application-11-of-12"></a>Implementar la herencia con Entity Framework 6 en una aplicación de ASP.NET MVC 5 (11 de 12)
====================
por [Tom Dykstra](https://github.com/tdykstra)

[Descargue el proyecto completado](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)

> La aplicación web de Contoso University muestra cómo crear aplicaciones de ASP.NET MVC 5 con Entity Framework 6 Code First y Visual Studio. Para obtener información sobre la serie de tutoriales, consulte [el primer tutorial de la serie](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


En el tutorial anterior controla las excepciones de simultaneidad. En este tutorial se muestra cómo implementar la herencia en el modelo de datos.

En la programación orientada a objetos, puede usar [herencia](http://en.wikipedia.org/wiki/Inheritance_(object-oriented_programming)) para facilitar la [reutilización del código](http://en.wikipedia.org/wiki/Code_reuse). En este tutorial, cambiará las clases `Instructor` y `Student` para que deriven de una clase base `Person` que contenga propiedades como `LastName`, que son comunes tanto para los instructores como para los alumnos. No tendrá que agregar ni cambiar ninguna página web, sino que cambiará parte del código y esos cambios se reflejarán automáticamente en la base de datos.

## <a name="options-for-mapping-inheritance-to-database-tables"></a>Opciones para asignar herencia a las tablas de bases de datos

El `Instructor` y `Student` clases en el `School` modelo de datos tienen varias propiedades que son idénticas:

![Student_and_Instructor_classes](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

Imagine que quiere eliminar el código redundante de las propiedades que comparten las entidades `Instructor` y `Student`. O que quiere escribir un servicio que pueda dar formato a los nombres sin preocuparse de si el nombre es de un instructor o de un alumno. Puede crear un `Person` clase base que contiene solo las propiedades compartidas y, después, realizar la `Instructor` y `Student` entidades hereden de esa clase base, como se muestra en la ilustración siguiente:

![Student_and_Instructor_classes_deriving_from_Person_class](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

Esta estructura de herencia se puede representar de varias formas en la base de datos. Podría tener un `Person` tabla que incluye información acerca de los estudiantes y profesores en una sola tabla. Algunas de las columnas podrían aplicarse solo a los instructores (`HireDate`), otras solo a los alumnos (`EnrollmentDate`), algunos en ambas (`LastName`, `FirstName`). Por lo general, tendría un *discriminador* columna para indicar qué tipo de cada fila representa. Por ejemplo, en la columna discriminadora podría aparecer "Instructor" para los instructores y "Student" para los alumnos.

![Tabla por hierarchy_example](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

Este patrón de generar una estructura de herencia de entidad de una tabla de base de datos única se denomina *tabla por jerarquía* herencia (TPH).

Una alternativa consiste en hacer que la base de datos se parezca más a la estructura de herencia. Por ejemplo, podría tener solo los campos de nombre el `Person` de tabla y tenga distintos `Instructor` y `Student` tablas con los campos de fecha.

![Tabla por type_inheritance](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Este patrón de hacer que una tabla de base de datos para cada clase de entidad se denomina *tabla por tipo* herencia (TPT).

Y todavía otra opción pasa por asignar todos los tipos no abstractos a tablas individuales. Todas las propiedades de una clase, incluidas las propiedades heredadas, se asignan a columnas de la tabla correspondiente. Este patrón se denomina herencia de tabla por clase concreta (TPC). Si se implementó la herencia de TPC para el `Person`, `Student`, y `Instructor` clases tal como se muestra anteriormente, el `Student` y `Instructor` tablas tendría el aspecto no diferentes después de que lo hacían antes de implementar la herencia.

TPC y patrones de herencia de TPH acostumbran un mejor rendimiento en Entity Framework que patrones de herencia de TPT, porque estos pueden provocar consultas de combinaciones complejas.

Este tutorial muestra cómo implementar la herencia de TPH. TPH es el patrón de herencia predeterminado en Entity Framework, por lo que todo lo que debe hacer es crear un `Person` clase, cambie el `Instructor` y `Student` clases se deriven de `Person`, agregue la nueva clase a la `DbContext`y crear un migración. (Para obtener información sobre cómo implementar otros patrones de herencia, vea [asignar la herencia de tabla por tipo (TPT)](https://msdn.microsoft.com/data/jj591617#2.5) y [asignar la herencia de la clase concreta por tabla (TPC)](https://msdn.microsoft.com/data/jj591617#2.6) en MSDN Documentación de Entity Framework.)

## <a name="create-the-person-class"></a>Creación de la clase Person

En el *modelos* carpeta, cree *Person.cs* y reemplace el código de plantilla con el código siguiente:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

## <a name="make-student-and-instructor-classes-inherit-from-person"></a>Asegúrese de que las clases Student e Instructor heredan de Person

En *Instructor.cs*, derivar el `Instructor` clase desde el `Person` clase y quite los campos claves y nombres. El código tendrá un aspecto similar al ejemplo siguiente:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Realizar modificaciones similares a *Student.cs*. La `Student` clase tendrá un aspecto similar al ejemplo siguiente:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

## <a name="add-the-person-entity-type-to-the-model"></a>Agregar el tipo de entidad Person al modelo

En *SchoolContext.cs*, agregue un `DbSet` propiedad para el `Person` tipo de entidad:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

Esto es todo lo que Entity Framework necesita para configurar la herencia de tabla por jerarquía. Como verá, cuando se actualiza la base de datos, tendrá una `Person` de tabla en lugar de la `Student` y `Instructor` tablas.

## <a name="create-and-update-a-migrations-file"></a>Crear y actualizar un archivo de migraciones

En la consola de administrador de paquetes (PMC), escriba el siguiente comando:

`Add-Migration Inheritance`

Ejecute el `Update-Database` comando en la PMC. El comando fallará en este momento porque tenemos las migraciones no sabe cómo tratar los datos existentes. Obtenga un mensaje de error como el siguiente:

> *No se pudo quitar el objeto ' dbo. Instructor' porque se hace referencia a una restricción FOREIGN KEY.*


Abra *migraciones\&lt; marca de tiempo&gt;\_Inheritance.cs* y reemplace el `Up` método con el código siguiente:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

Este código se encarga de las siguientes tareas de actualización de la base de datos:

- Quita las restricciones de la clave externa y los índices que apuntan a la tabla Student.
- Cambia el nombre de la tabla Instructor por Person y realiza los cambios necesarios para que pueda almacenar datos de los alumnos:

    - Agrega EnrollmentDate que acepta valores NULL para alumnos.
    - Agrega la columna discriminadora para indicar si una fila es para un alumno o para un instructor.
    - Hace que HireDate admita un valor NULL, puesto que las filas de alumnos no dispondrán de fechas de contratación.
    - Agrega un campo temporal que se usará para actualizar las claves externas que apuntan a los alumnos. Cuando copie alumnos en la tabla Person obtendrán nuevos valores de clave principales.
- Copia datos de la tabla Student a la tabla Person. Esto hace que los alumnos se asignen a nuevos valores de clave principal.
- Corrige los valores de clave externa correspondientes a los alumnos.
- Vuelve a crear las restricciones y los índices de la clave externa, pero ahora los dirige a la tabla Person.

(Si hubiera usado el GUID en lugar de un número entero como tipo de clave principal, los valores de la clave principal de alumno no tendrían que cambiar y algunos de estos pasos se podrían haber omitido).

Ejecute el `update-database` nuevo comando.

(En un sistema de producción provocará que los cambios correspondientes en el método Down en caso de que alguna vez ha tenido usar para volver a la versión anterior de la base de datos. En este tutorial no va a usar el método Down.)

> [!NOTE]
> Es posible generar otros errores al migrar datos y realizar cambios de esquema. Si se producen errores de migración no puede resolver, puede continuar con el tutorial, cambie la cadena de conexión en el *Web.config* de archivo o la base de datos mediante la eliminación. El enfoque más sencillo consiste en cambiar el nombre de la base de datos en el *Web.config* archivo. Por ejemplo, cambiar el nombre de la base de datos a ContosoUniversity2 tal como se muestra en el ejemplo siguiente:
>
> [!code-xml[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.xml?highlight=2)]
>
> Con una base de datos, no hay ningún dato para migrar y el `update-database` comando es mucho más probable que se completará sin errores. Para obtener instrucciones sobre cómo eliminar la base de datos, consulte [cómo quitar una base de datos de Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/). Si adopta este enfoque para poder continuar con el tutorial, omita el paso de implementación al final de este tutorial o implementar un nuevo sitio y la base de datos. Si implementa una actualización en el mismo sitio que ha sido implementando ya, EF obtendrá el mismo error cuando se ejecutan automáticamente las migraciones. Si desea solucionar un error de las migraciones, el mejor recurso es uno de los foros de Entity Framework o StackOverflow.com.


## <a name="testing"></a>Pruebas

Ejecute el sitio Web y prueba en distintas páginas. Todo funciona igual que antes.

En **Explorador de servidores,** expanda **datos Connections\SchoolContext** y, a continuación, **tablas**, y verá que el **estudiante** y **Instructor** tablas han sido reemplazadas por un **persona** tabla. Expanda el **persona** tabla y verá que tiene todas las columnas que solían haber en el **estudiante** y **Instructor** tablas.

![Server_Explorer_showing_Person_table](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

Haga clic con el botón derecho en la tabla Person y después haga clic en **Mostrar datos de tabla** para ver la columna discriminadora.

![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

El siguiente diagrama muestra la estructura de la nueva base de datos School:

![School_database_diagram](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

## <a name="deploy-to-azure"></a>Implementar en Azure

En esta sección, deberá haber completado opcional **implementar la aplicación en Azure** sección [parte 3, ordenación, filtrado y paginación](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md) de esta serie de tutoriales. Si tenía errores de las migraciones que puede resolver mediante la eliminación de la base de datos en su proyecto local, omita este paso; o crear un nuevo sitio y la base de datos e implementar en el nuevo entorno.

1. En Visual Studio, haga clic en el proyecto en **el Explorador de soluciones** y seleccione **publicar** en el menú contextual.

    ![Publicar en el menú contextual del proyecto](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)
2. Haga clic en **Publicar**.

    ![publicar](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

   La aplicación Web se abrirá en el explorador predeterminado.
3. Probar la aplicación para comprobar funciona.

    La primera vez que ejecute una página que tiene acceso a la base de datos, Entity Framework se ejecuta todas las migraciones `Up` métodos necesarios para la base de datos al día con el modelo de datos actual.

## <a name="summary"></a>Resumen

Ha implementado la herencia de tabla por jerarquía en las clases `Person`, `Student` y `Instructor`. Para obtener más información acerca de ésta y otras estructuras de herencia, vea [patrón de herencia de TPT](https://msdn.microsoft.com/data/jj618293) y [patrón de herencia de TPH](https://msdn.microsoft.com/data/jj618292) en MSDN. En el siguiente tutorial, aprenderá a controlar una serie de escenarios de Entity Framework relativamente avanzados.

Pueden encontrar vínculos a otros recursos de Entity Framework en el [acceso a datos de ASP.NET - recursos recomendados](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Anterior](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [Siguiente](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)
