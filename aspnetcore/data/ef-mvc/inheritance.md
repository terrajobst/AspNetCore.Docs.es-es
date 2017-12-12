---
title: "Núcleo de ASP.NET MVC con EF Core - herencia - 9 de 10"
author: tdykstra
description: "Este tutorial le mostrará cómo implementar la herencia en el modelo de datos con Entity Framework Core en una aplicación de ASP.NET Core."
keywords: Herencia de ASP.NET Core, Entity Framework Core,
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.assetid: 41dc0db7-6f17-453e-aba6-633430609c74
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/inheritance
ms.openlocfilehash: 10bde121dac3bdbbf0e55f2d146d91dea0f0210f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
# <a name="inheritance---ef-core-with-aspnet-core-mvc-tutorial-9-of-10"></a>Herencia - Core EF con el tutorial de MVC de ASP.NET Core (9 de 10)

Por [Tom Dykstra](https://github.com/tdykstra) y [Rick Anderson](https://twitter.com/RickAndMSFT)

La aplicación web de ejemplo de la Universidad de Contoso muestra cómo crear aplicaciones web de MVC de ASP.NET Core con Entity Framework Core y Visual Studio. Para obtener información acerca de la serie de tutoriales, vea [el primer tutorial de la serie](intro.md).

En el tutorial anterior, para administrar las excepciones de simultaneidad. Este tutorial le mostrará cómo implementar la herencia en el modelo de datos.

En la programación orientada a objetos, puede usar la herencia para facilitar la reutilización del código. En este tutorial, va a cambiar la `Instructor` y `Student` clases para que derivan de un `Person` clase que contiene propiedades como base `LastName` que son comunes a los instructores y alumnos. No agregar ni cambiar todas las páginas web, pero podrá cambiar parte del código y esos cambios se reflejarán automáticamente en la base de datos.

## <a name="options-for-mapping-inheritance-to-database-tables"></a>Opciones para la asignación de herencia para las tablas de base de datos

El `Instructor` y `Student` las clases en el modelo de datos School tienen varias propiedades que son idénticas:

![Clases de Student y Instructor](inheritance/_static/no-inheritance.png)

Imagine que desea eliminar el código redundante para las propiedades que comparten el `Instructor` y `Student` entidades. O bien, desea escribir un servicio que puede dar formato a los nombres sin preocuparse de si el nombre procede de un instructor o un estudiante. Puede crear un `Person` clase base que contiene solo las propiedades comparten, realice la `Instructor` y `Student` clases heredan de esa clase base, como se muestra en la siguiente ilustración:

![Student y Instructor clases derivadas de la clase de persona](inheritance/_static/inheritance.png)

Hay varias maneras de que esta estructura de herencia se podría representar en la base de datos. Podría tener una tabla de la persona que incluye información acerca de los alumnos e instructores en una sola tabla. Algunas de las columnas pudieron aplicar solo a instructores (HireDate), algunas solo a los alumnos (EnrollmentDate), algunos para ambos (LastName, FirstName). Por lo general, tendría una columna discriminadora para indicar qué tipo de cada fila representa. Por ejemplo, la columna discriminadora podría tener "Instructor" para instructores y "Student" para estudiantes.

![Ejemplo de tabla por jerarquía](inheritance/_static/tph.png)

Este patrón consiste en generar una estructura de herencia de la entidad de una tabla de base de datos único se denomina herencia de tabla por jerarquía (TPH).

Una alternativa consiste en hacer que la base de datos se parecen más a la estructura de herencia. Por ejemplo, podría tener sólo los campos de nombre de la tabla Person y tener tablas Instructor y Student independientes con los campos de fecha.

![Herencia de tabla por tipo](inheritance/_static/tpt.png)

Este patrón de crear una tabla de base de datos para cada clase de entidad se denomina tabla por herencia de tipo (TPT).

Todavía otra opción es asignar todos los tipos de no abstractas para tablas individuales. Todas las propiedades de una clase, incluidas las propiedades heredadas, se asignan a columnas de la tabla correspondiente. Este patrón se denomina herencia de clases de tabla por concreto (TCP). Si implementa la herencia de TCP para las clases de persona y estudiantes, Instructor tal como se muestra anteriormente, las tablas Student y Instructor tendría un aspecto diferentes no después de implementar la herencia que ocupaban antes.

TPC y TPH patrones de herencia generalmente entregar un mejor rendimiento que los patrones de herencia de TPT, porque pueden dar lugar a patrones TPT en consultas de combinaciones complejas.

Este tutorial muestra cómo implementar la herencia TPH. TPH es el patrón de herencia única que admite el núcleo de Entity Framework.  Lo que deberá hacer es crear un `Person` clase, cambie el `Instructor` y `Student` clases que derivan de `Person`, agregar la nueva clase a la `DbContext`y crear una migración.

> [!TIP] 
> Considere la posibilidad de guardar una copia del proyecto antes de realizar los siguientes cambios.  A continuación, si experimenta problemas y necesidad de volver a empezar, le resultará más fácil iniciar desde el proyecto guardado en lugar de invertir los pasos que se realiza en este tutorial o que se va hacia el principio de la serie completa.

## <a name="create-the-person-class"></a>Crear la clase de persona

En la carpeta Models, crear Person.cs y reemplace el código de plantilla con el código siguiente:

[!code-csharp[Main](intro/samples/cu/Models/Person.cs)]

## <a name="make-student-and-instructor-classes-inherit-from-person"></a>Asegúrese de Student y Instructor clases heredan de persona

En *Instructor.cs*, derive la clase Instructor de la clase de persona y quitar los campos claves y nombres. El código tendrá un aspecto similar al ejemplo siguiente:

[!code-csharp[Main](intro/samples/cu/Models/Instructor.cs?name=snippet_AfterInheritance&highlight=8)]

Realizar los mismos cambios en *Student.cs*.

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_AfterInheritance&highlight=8)]

## <a name="add-the-person-entity-type-to-the-data-model"></a>Agregar el tipo de entidad de la persona en el modelo de datos

Agregar el tipo de entidad de la persona a *SchoolContext.cs*. Se resaltan las líneas nuevas.

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_AfterInheritance&highlight=19,30)]

Esto es todo lo que necesita de Entity Framework para configurar la herencia de tabla por jerarquía. Como verá, cuando se actualiza la base de datos, tendrá una tabla Person en lugar de las tablas Student y Instructor.

## <a name="create-and-customize-migration-code"></a>Crear y personalizar el código de la migración

Guarde los cambios y compile el proyecto. A continuación, abra la ventana de comandos en la carpeta de proyecto y escriba el siguiente comando:

```console
dotnet ef migrations add Inheritance
```

No se ejecutan los `database update` comando todavía. Este comando dará como resultado pérdida de datos debido a que elimine la tabla Instructor y cambiar el nombre de la tabla de estudiantes en persona. Deberá proporcionar código personalizado para conservar los datos existentes.

Abra *migraciones /\<marca de tiempo > _Inheritance.cs* y reemplace el `Up` método por el código siguiente:

[!code-csharp[Main](intro/samples/cu/Migrations/20170216215525_Inheritance.cs?name=snippet_Up)]

Este código se encarga de las tareas de actualización de base de datos siguientes:

* Quita las restricciones foreign key y los índices que apuntan a la tabla de estudiantes.

* Cambia el nombre de la tabla Instructor como persona y realiza cambios necesarios almacenar datos de los alumnos:

* Agrega EnrollmentDate que aceptan valores NULL para estudiantes.

* Agrega la columna discriminadora para indicar si una fila es para un estudiante o un instructor.

* Hace HireDate que aceptan valores NULL ya que las filas de estudiantes no tengan las fechas de contratación.

* Agrega un campo temporal que se usará para actualizar las claves externas que apuntan a los alumnos. Cuando copie estudiantes en la tabla Person obtendrá nuevos valores de clave principales.

* Copia datos de la tabla de estudiante en la tabla Person. Esto hace que los alumnos se asignen nuevos valores de clave principales.

* Corrige los valores de clave externa que señalan a los alumnos.

* Vuelve a crear las restricciones foreign key y los índices, ahora que apunta a la tabla Person.

(Si hubiera usado el GUID en lugar de entero como el tipo de clave principal, los valores de clave principal de estudiante no tienen que cambiar y algunos de estos pasos se ha omitido).

Ejecute el `database update` comando:

```console
dotnet ef database update
```

(En un sistema de producción haría que los cambios correspondientes en el `Down` método en caso de que alguna vez ha tenido que usar para volver a la versión anterior de la base de datos. Para este tutorial no va a usar el `Down` método.)

> [!NOTE] 
> Es posible obtener otros errores al realizar cambios de esquema en una base de datos con los datos existentes. Si se producen errores de migración que no se puede resolver, puede cambiar el nombre de la base de datos en la cadena de conexión o eliminar la base de datos. Con una base de datos, no hay ningún dato para migrar y el comando de actualización de base de datos es más probable que se completan sin errores. Para eliminar la base de datos, utilice SSOX o ejecute la `database drop` comando de CLI.

## <a name="test-with-inheritance-implemented"></a>Probar con herencia implementada

Ejecute la aplicación e intente distintas páginas. El mismo todo funciona igual que antes.

En **Explorador de objetos de SQL Server**, expanda **las conexiones de datos/SchoolContext** y, a continuación, **tablas**, y verá que las tablas Student y Instructor se han reemplazado por un Tabla Person. Abra el Diseñador de tablas de la persona y puede ver que tiene todas las columnas que solían haber en las tablas Student y Instructor.

![Tabla Person en SSOX](inheritance/_static/ssox-person-table.png)

Haga clic en la tabla Person y, a continuación, haga clic en **mostrar datos de tabla** para ver la columna discriminadora.

![Tabla Person en SSOX - datos de la tabla](inheritance/_static/ssox-person-data.png)

## <a name="summary"></a>Resumen

Ha implementado la herencia de tabla por jerarquía para el `Person`, `Student`, y `Instructor` clases. Para obtener más información acerca de la herencia en Entity Framework Core, vea [herencia](https://docs.microsoft.com/ef/core/modeling/inheritance). En el siguiente tutorial verá cómo controlar una variedad de escenarios de Entity Framework relativamente avanzados.

>[!div class="step-by-step"]
[Anterior](concurrency.md)
[Siguiente](advanced.md)  
