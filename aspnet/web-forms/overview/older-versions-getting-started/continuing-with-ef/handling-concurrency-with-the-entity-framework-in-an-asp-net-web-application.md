---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application
title: Control de simultaneidad con Entity Framework 4.0 en una aplicación de ASP.NET 4 Web | Documentos de Microsoft
author: tdykstra
description: Esta serie de tutoriales se basa en la aplicación web de la Universidad de Contoso que se crea mediante la introducción a la serie de tutoriales de Entity Framework 4.0. I...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2011
ms.topic: article
ms.assetid: a5aa22a6-fb7f-4f41-9c7f-addda151940b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application
msc.type: authoredcontent
ms.openlocfilehash: f40695270006e4f8b0c9ad8e94049e5239f06e63
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30889867"
---
<a name="handling-concurrency-with-the-entity-framework-40-in-an-aspnet-4-web-application"></a>Control de simultaneidad con Entity Framework 4.0 en una aplicación Web 4 de ASP.NET
====================
por [Tom Dykstra](https://github.com/tdykstra)

> Esta serie de tutoriales que se basa en la aplicación web de la Universidad de Contoso que se crea mediante la [Getting Started with the Entity Framework 4.0](https://asp.net/entity-framework/tutorials#Getting%20Started) serie de tutoriales. Si no has completado los tutoriales anteriores, como punto de partida para este tutorial puede [descargar la aplicación](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) que puede haberla creado. También puede [descargar la aplicación](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) creado por la serie de tutoriales completa. Si tiene alguna pregunta acerca de los tutoriales, puede publicar para la [foro de ASP.NET Entity Framework](https://forums.asp.net/1227.aspx).


En el tutorial anterior, aprendió cómo ordenar y filtrar datos mediante el `ObjectDataSource` control y Entity Framework. Este tutorial muestra opciones para controlar la simultaneidad en una aplicación web ASP.NET que utiliza Entity Framework. Se creará una nueva página web que se dedica a actualizar las asignaciones de office de instructor. También podrá controlar problemas de simultaneidad en esa página y en la página de departamentos que creó anteriormente.

[![Image06](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image2.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image1.png)

[![Image01](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image4.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image3.png)

## <a name="concurrency-conflicts"></a>Conflictos de simultaneidad

Cuando un usuario edita un registro y otro usuario modifica el mismo registro antes de que el primer cambio del usuario se escribe en la base de datos, se produce un conflicto de simultaneidad. Si no configura el Entity Framework para detectar estos conflictos, persona que actualiza la base de datos en último lugar sobrescribe los cambios del otro usuario. En muchas aplicaciones, el riesgo es aceptable y no tendrá que configurar la aplicación para controlar los posibles conflictos de simultaneidad. (Si hay determinados usuarios o algunas de las actualizaciones, o si no es realmente crítica si se sobrescriben algunos cambios, el costo de la programación para la simultaneidad puede sobrepasar el beneficio obtenido.) Si no tiene que preocuparse sobre los conflictos de simultaneidad, puede omitir este tutorial; los dos tutoriales restantes de la serie no dependen de todo lo que se genera en este.

### <a name="pessimistic-concurrency-locking"></a>Simultaneidad pesimista (bloqueo)

Si la aplicación necesita evitar la pérdida accidental de datos en casos de simultaneidad, una manera de hacerlo es usar los bloqueos de base de datos. Esto se denomina *simultaneidad pesimista*. Por ejemplo, antes de leer una fila de una base de datos, solicita un bloqueo de solo lectura o para acceso de actualización. Si bloquea una fila para acceso de actualización, no se permite que ningún otro usuario bloquee la fila como solo lectura o para acceso de actualización, porque recibirían una copia de los datos que se están modificando. Si bloquea una fila para acceso de solo lectura, otras personas también pueden bloquearla para acceso de solo lectura pero no para actualización.

Administrar bloqueos tiene algunas desventajas. Puede ser bastante complicado de programar. Se necesitan recursos de administración de base de datos, y puede provocar problemas de rendimiento como el número de usuarios de una aplicación aumenta (es decir, no se escala bien). Por estos motivos, no todos los sistemas de administración de bases de datos admiten la simultaneidad pesimista. Entity Framework no proporciona ninguna compatibilidad integrada para él, y este tutorial no muestra cómo se implementa.

### <a name="optimistic-concurrency"></a>Simultaneidad optimista

La alternativa a la simultaneidad pesimista es *simultaneidad optimista*. La simultaneidad optimista implica permitir que se produzcan conflictos de simultaneidad y reaccionar correctamente si ocurren. Por ejemplo, Juan ejecuta el *Department.aspx* página, clics el **editar** de vínculos para el departamento de historial y reduce la **presupuesto** cantidad de 1.000.000,00 $ $ 125,000.00. (Juan administra un departamento competencia y desea liberar dinero para su propio departamento).

[![Image07](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image6.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image5.png)

Antes de que Juan hace clic en **actualización**, Julia se ejecuta la misma página, haga clic en el **editar** vínculo para el departamento de historial y, a continuación, cambios el **Start Date** campo de 1/10/2011 1/1 / 1999. (Julia administra el departamento de historial y desea asignarle más antigüedad).

[![Image08](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image8.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image7.png)

John hace clic en **actualización** en primer lugar, a continuación, hace clic Julia **actualización**. Listas de Julia explorador ahora la **presupuesto** importe como $1.000.000,00, pero esto es incorrecto porque la cantidad se ha modificado por John a 125,000.00 $.

Algunas de las acciones que puede realizar en este escenario son las siguientes:

- Puede realizar un seguimiento de la propiedad que ha modificado un usuario y actualizar solo las columnas correspondientes de la base de datos. En el escenario de ejemplo, no se perdería ningún dato porque los dos usuarios actualizaron diferentes propiedades. La próxima vez que un usuario examina el departamento de historial, podrán disfrutar de 1/1/1999 y $125,000.00. 

    Éste es el comportamiento predeterminado en Entity Framework y puede reducir considerablemente el número de conflictos que pueden dar lugar a pérdida de datos. Sin embargo, este comportamiento no evita la pérdida de datos si se realizan cambios competencia a la misma propiedad de una entidad. Además, este comportamiento no siempre es posible; Cuando los procedimientos almacenados se asignan a un tipo de entidad, todas las propiedades de una entidad se actualizan cuando se realizan cambios en la entidad en la base de datos.
- Puede permitir que los cambios de Julia sobrescribir los cambios de John. Después haga clic en Julia **actualización**, el **presupuesto** cantidad vuelve a 1.000.000,00 $. Esto se denomina un escenario de *Prevalece el cliente* o *Prevalece el último*. (Los valores del cliente tienen prioridad sobre lo que aparece en el almacén de datos.)
- Cambio de Julia puede impedir que se actualiza en la base de datos. Por lo general, debería mostrar un mensaje de error, mostrarle el estado actual de los datos y le permita volver a introducir sus cambios si todavía desea hacerlos. Para automatizar el proceso aún más pudo guardar su entrada y lo que le proporciona una oportunidad de volver a aplicar sin tener que volver a escribirlo. Esto se denomina un escenario de *Prevalece el almacén*. (Los valores del almacén de datos tienen prioridad sobre los valores enviados por el cliente).

### <a name="detecting-concurrency-conflicts"></a>Detectar conflictos de simultaneidad

En Entity Framework, puede resolver conflictos de control de `OptimisticConcurrencyException` las excepciones que inicia de Entity Framework. Para saber cuándo se producen dichas excepciones, Entity Framework debe ser capaz de detectar conflictos. Por lo tanto, debe configurar correctamente la base de datos y el modelo de datos. Algunas opciones para habilitar la detección de conflictos son las siguientes:

- En la base de datos incluye una columna de tabla que puede utilizarse para determinar cuándo se ha modificado una fila. A continuación, puede configurar el Entity Framework para que incluya esa columna en la `Where` cláusula de SQL `Update` o `Delete` comandos.

    Que es el propósito de la `Timestamp` columna en el `OfficeAssignment` tabla.

    [![Image09](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image10.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image9.png)

    Tipo de datos de la `Timestamp` columna también se denomina `Timestamp`. Sin embargo, la columna realmente no contiene un valor de fecha u hora. En su lugar, el valor es un número secuencial que se incrementa cada vez que se actualiza la fila. En un `Update` o `Delete` comando, el `Where` cláusula incluye original `Timestamp` valor. Si ha cambiado por otro usuario, el valor de la fila que se está actualiza `Timestamp` es diferente del valor original, por lo que el `Where` cláusula no devuelve ninguna fila para actualizar. Cuando Entity Framework detecta que se ha actualizado ninguna fila actualmente `Update` o `Delete` comando (es decir, cuando el número de filas afectadas es cero), que interpreta como un conflicto de simultaneidad.
- Configurar Entity Framework para incluir los valores originales de cada columna en la tabla en la `Where` cláusula de `Update` y `Delete` comandos.

    Como se muestra en la primera opción, si algo en la fila ha cambiado desde que se leyó primero la fila, el `Where` cláusula no devolverá una fila que se va a actualizar, que Entity Framework se interpreta como un conflicto de simultaneidad. Este método es tan eficaz como mediante un `Timestamp` campo, pero puede ser ineficaz. Para las tablas de base de datos que tienen muchas columnas, pueden ser muy grandes `Where` cláusulas, y en una aplicación web puede requerir que mantener grandes cantidades de estado. Mantenimiento de grandes cantidades de estado puede afectar al rendimiento de la aplicación porque requiere recursos del servidor (por ejemplo, el estado de sesión) o debe incluirse en la página web de sí mismo (por ejemplo, el estado de vista).

En este tutorial va a agregar el control de errores de conflictos de simultaneidad optimista para una entidad que no tiene una propiedad de seguimiento (la `Department` entidad) y para una entidad que tiene una propiedad de seguimiento (el `OfficeAssignment` entidad).

## <a name="handling-optimistic-concurrency-without-a-tracking-property"></a>Control de simultaneidad optimista sin una propiedad de seguimiento

Para implementar la simultaneidad optimista para la `Department` entidad, que no tiene un seguimiento (`Timestamp`) propiedad, se realizarán las siguientes tareas:

- Cambiar el modelo de datos para habilitar el seguimiento para simultaneidad `Department` entidades.
- En el `SchoolRepository` (clase), controlar las excepciones de simultaneidad en el `SaveChanges` método.
- En el *Departments.aspx* controlar las excepciones de simultaneidad, mostrando un mensaje al usuario que los cambios intentados eran incorrectos de advertencia de la página. El usuario puede ver los valores actuales y vuelva a intentar los cambios si todavía son necesarios.

### <a name="enabling-concurrency-tracking-in-the-data-model"></a>Habilitar el seguimiento en el modelo de datos de simultaneidad

En Visual Studio, abra la aplicación web de la Universidad de Contoso que estaba trabajando con en el tutorial anterior de esta serie.

Abra *SchoolModel.edmx*y en el Diseñador de modelos de datos, haga clic en el `Name` propiedad en el `Department` entidad y, a continuación, haga clic en **propiedades**. En el **propiedades** ventana, cambiar la `ConcurrencyMode` propiedad `Fixed`.

[![Image16](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image12.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image11.png)

Haga lo mismo para las demás propiedades escalares no son de clave principal (`Budget`, `StartDate`, y `Administrator`.) (No puede hacerlo para las propiedades de navegación.) Esto especifica que cada vez que Entity Framework genera un `Update` o `Delete` comando SQL que se va a actualizar el `Department` entidad en la base de datos, estas columnas (con valores originales) deben incluirse en la `Where` cláusula. Si se encuentra ninguna fila cuando la `Update` o `Delete` se ejecuta el comando, Entity Framework se iniciará una excepción de simultaneidad optimista.

Guarde y cierre el modelo de datos.

### <a name="handling-concurrency-exceptions-in-the-dal"></a>Controlar las excepciones de simultaneidad en la capa DAL

Abra *SchoolRepository.cs* y agregue el siguiente `using` instrucción para el `System.Data` espacio de nombres:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample1.cs)]

Agregue las siguientes nuevas `SaveChanges` método, que controla las excepciones de simultaneidad optimista:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample2.cs)]

Si se produce un error de simultaneidad cuando se llama a este método, los valores de propiedad de la entidad en la memoria se reemplazan por los valores actuales de la base de datos. Para que la página web puede controlarla, se vuelve a producir la excepción de simultaneidad.

En el `DeleteDepartment` y `UpdateDepartment` métodos, reemplace la llamada existente a `context.SaveChanges()` con una llamada a `SaveChanges()` para invocar el método nuevo.

### <a name="handling-concurrency-exceptions-in-the-presentation-layer"></a>Controlar las excepciones de simultaneidad en el nivel de presentación

Abra *Departments.aspx* y agregue un `OnDeleted="DepartmentsObjectDataSource_Deleted"` atribuir a la `DepartmentsObjectDataSource` control. La etiqueta de apertura del control ahora parecerán al ejemplo siguiente.

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample3.aspx)]

En el `DepartmentsGridView` controlar, especifique todas las columnas de tabla en el `DataKeyNames` de atributo, como se muestra en el ejemplo siguiente. Tenga en cuenta que esto creará una vista muy grande campos de estado, que es uno de los motivos por qué usar un campo de seguimiento suele ser la mejor manera de realizar el seguimiento de conflictos de simultaneidad.

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample4.aspx)]

Abra *Departments.aspx.cs* y agregue el siguiente `using` instrucción para el `System.Data` espacio de nombres:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample5.cs)]

Agregue el siguiente método nuevo que llamará desde el control de origen de datos `Updated` y `Deleted` controladores de eventos para controlar las excepciones de simultaneidad:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample6.cs)]

Este código comprueba el tipo de excepción y, si es una excepción de simultaneidad, el código crea dinámicamente un `CustomValidator` control que a su vez muestra un mensaje en el `ValidationSummary` control.

Llame al método nuevo desde el `Updated` controlador de eventos que agregó anteriormente. Además, cree un nuevo `Deleted` controlador de eventos que llama al mismo método (pero no hace nada más):

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample7.cs)]

### <a name="testing-optimistic-concurrency-in-the-departments-page"></a>Probar la simultaneidad optimista en la página de departamentos

Ejecute el *Departments.aspx* página.

[![Image17](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image14.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image13.png)

Haga clic en **editar** en una fila y cambie el valor en el **presupuesto** columna. (Recuerde que solo se pueden editar los registros que ha creado para este tutorial, porque las existentes `School` registros de base de datos contienen algunos datos no válidos. El registro para el departamento de ahorro es una prueba de errores para experimentar con).

[![Image18](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image16.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image15.png)

Abra una nueva ventana del explorador y ejecute la página nuevo (copiar la dirección URL del cuadro de dirección de la primera ventana Explorador a la segunda ventana del explorador).

[![Image17](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image18.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image17.png)

Haga clic en **editar** en la misma fila que editó anteriormente y cambiar la **presupuesto** valor a algo distinto.

[![Image19](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image20.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image19.png)

En la segunda ventana del explorador, haga clic en **actualización**. El **presupuesto** cantidad se cambió correctamente a este nuevo valor.

[![Image20](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image22.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image21.png)

En la primera ventana de explorador, haga clic en **actualización**. Se produce un error en la actualización. El **presupuesto** cantidad se vuelve a mostrar con el valor establecido en la segunda ventana del explorador y verá un mensaje de error.

[![Image21](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image24.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image23.png)

## <a name="handling-optimistic-concurrency-using-a-tracking-property"></a>Control de simultaneidad optimista con una propiedad de seguimiento

Para controlar la simultaneidad optimista para una entidad que tiene una propiedad de seguimiento, se realizarán las siguientes tareas:

- Agregar procedimientos almacenados al modelo de datos para administrar `OfficeAssignment` entidades. (Propiedades de seguimiento y los procedimientos almacenados no tienen que usarse conjuntamente; están solo agrupen aquí a modo de ilustración).
- Agregar métodos CRUD a la capa DAL y BLL para `OfficeAssignment` entidades, incluido el código para controlar las excepciones de simultaneidad optimista en la capa DAL.
- Crear una página web de las asignaciones de oficina.
- Probar la simultaneidad optimista en la nueva página web.

### <a name="adding-officeassignment-stored-procedures-to-the-data-model"></a>Agregar OfficeAssignment los procedimientos almacenados para el modelo de datos

Abra la *SchoolModel.edmx* un archivo en el Diseñador de modelos, haga clic en la superficie de diseño y haga clic en **actualizar modelo desde base de datos**. En el **agregar** pestaña de la **elija los objetos de base de datos** cuadro de diálogo, expanda **procedimientos almacenados** y seleccione los tres `OfficeAssignment` procedimientos almacenados (vea el siguiendo la captura de pantalla) y, a continuación, haga clic en **finalizar**. (Estos procedimientos almacenados ya estaban en la base de datos cuando se descarga o creó mediante una secuencia de comandos.)

[![Image02](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image26.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image25.png)

Haga clic en el `OfficeAssignment` entidad y seleccione **Stored Procedure Mapping**.

[![Image03](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image28.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image27.png)

Establecer el **insertar**, **actualización**, y **eliminar** procedimientos almacenan de funciones que puede usar sus correspondientes. Para el `OrigTimestamp` parámetro de la `Update` función, establezca el **propiedad** a `Timestamp` y seleccione la **valor Original de uso** opción.

[![Image04](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image30.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image29.png)

Cuando se llama el marco de trabajo de la entidad la `UpdateOfficeAssignment` procedimiento almacenado, pasará el valor original de la `Timestamp` columna en el `OrigTimestamp` parámetro. El procedimiento almacenado usa este parámetro en su `Where` cláusula:

[!code-sql[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample8.sql)]

El procedimiento almacenado también selecciona el nuevo valor de la `Timestamp` columna después de la actualización para que Entity Framework puede mantener el `OfficeAssignment` entidad que está en la memoria sincronizada con la fila correspondiente de la base de datos.

(Tenga en cuenta que el procedimiento almacenado para eliminar una asignación de office no tiene un `OrigTimestamp` parámetro. Por este motivo, Entity Framework no se puede comprobar que una entidad no se ha cambiado antes de eliminarlo.)

Guarde y cierre el modelo de datos.

### <a name="adding-officeassignment-methods-to-the-dal"></a>Agregar métodos OfficeAssignment a la capa DAL

Abra *ISchoolRepository.cs* y agregue los siguientes métodos CRUD para la `OfficeAssignment` conjunto de entidades:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample9.cs)]

Agregue los siguientes métodos nuevos a *SchoolRepository.cs*. En el `UpdateOfficeAssignment` método, se llama a la variable local `SaveChanges` en lugar del método `context.SaveChanges`.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample10.cs)]

En el proyecto de prueba, abra *MockSchoolRepository.cs* y agregue el siguiente `OfficeAssignment` colección y métodos CRUD a él. (El repositorio ficticio debe implementar la interfaz del repositorio, o no se compilará la solución).

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample11.cs)]

### <a name="adding-officeassignment-methods-to-the-bll"></a>Agregar métodos OfficeAssignment a la capa BLL

En el proyecto principal, abra *SchoolBL.cs* y agregue los siguientes métodos CRUD para el `OfficeAssignment` del conjunto de entidades a ella:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample12.cs)]

## <a name="creating-an-officeassignments-web-page"></a>Crear una página de Web OfficeAssignments

Crear una nueva página web que usa el *Site.Master* página principal y asígnele el nombre *OfficeAssignments.aspx*. Agregue el siguiente marcado para la `Content` control denominado `Content2`:

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample13.aspx)]

Tenga en cuenta que en el `DataKeyNames` de atributo, el marcado especifica la `Timestamp` propiedad, así como la clave de registro (`InstructorID`). Especificar propiedades en el `DataKeyNames` atributo hace que el control se guarden en el estado del control (que es similar al estado de vista) para que estén disponibles los valores originales durante el procesamiento de devolución de datos.

Si no guardó el `Timestamp` valor, Entity Framework no tendría como parte de la `Where` cláusula de la instrucción SQL `Update` comando. Por lo tanto, nada pudieran encontrarse para actualizar. Como resultado, Entity Framework produciría una excepción de simultaneidad optimista cada vez un `OfficeAssignment` se actualiza la entidad.

Abra *OfficeAssignments.aspx.cs* y agregue el siguiente `using` instrucción para la capa de acceso a datos:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample14.cs)]

Agregue las siguientes `Page_Init` método, que habilita la funcionalidad de datos dinámicos. También agregue el siguiente controlador para el `ObjectDataSource` del control `Updated` eventos con el fin de comprobar si hay errores de simultaneidad:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample15.cs)]

### <a name="testing-optimistic-concurrency-in-the-officeassignments-page"></a>Probar la simultaneidad optimista en la página de OfficeAssignments

Ejecute el *OfficeAssignments.aspx* página.

[![Image10](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image32.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image31.png)

Haga clic en **editar** en una fila y cambie el valor en el **ubicación** columna.

[![image11](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image34.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image33.png)

Abra una nueva ventana del explorador y ejecute la página nuevo (copiar la dirección URL de la primera ventana de explorador a la segunda ventana del explorador).

[![Image10](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image36.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image35.png)

Haga clic en **editar** en la misma fila que editó anteriormente y cambiar la **ubicación** valor a algo distinto.

[![Image12](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image38.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image37.png)

En la segunda ventana del explorador, haga clic en **actualización**.

[![Image13](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image40.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image39.png)

Cambie a la primera ventana del explorador y haga clic en **actualización**.

[![Image15](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image42.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image41.png)

Verá un mensaje de error y la **ubicación** valor ha sido actualizado para mostrar el valor que ha cambiado en la segunda ventana del explorador.

## <a name="handling-concurrency-with-the-entitydatasource-control"></a>Control de simultaneidad con el Control EntityDataSource

El `EntityDataSource` control incluye lógica integrada que reconoce la configuración de simultaneidad en el modelo de datos y controladores de actualización y las operaciones de eliminación en consecuencia. Sin embargo, al igual que con todas las excepciones, debe controlar `OptimisticConcurrencyException` excepciones usted mismo con el fin de proporcionar un mensaje de error descriptivo.

A continuación, configurará la *Courses.aspx* página (que usa un `EntityDataSource` control) para permitir la actualización y las operaciones de eliminación y mostrar un mensaje de error si se produce un conflicto de simultaneidad. El `Course` entidad no tiene una simultaneidad seguimiento de columna, por lo que usará el mismo método que se hacía con la `Department` entidad: realizar un seguimiento de los valores de todas las propiedades no son de clave.

Abra la *SchoolModel.edmx* archivo. Para las propiedades no son de clave de la `Course` entidad (`Title`, `Credits`, y `DepartmentID`), establezca el **modo de simultaneidad** propiedad `Fixed`. A continuación, guarde y cierre el modelo de datos.

Abra la *Courses.aspx* página y realice los cambios siguientes:

- En el `CoursesEntityDataSource` de control, agregue `EnableUpdate="true"` y `EnableDelete="true"` atributos. La etiqueta de apertura de ese control ahora es similar al siguiente:

    [!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample16.aspx)]
- En el `CoursesGridView` controlar, cambie la `DataKeyNames` valor al atributo `"CourseID,Title,Credits,DepartmentID"`. A continuación, agregue un `CommandField` elemento a la `Columns` elemento que muestra **editar** y **eliminar** botones (`<asp:CommandField ShowEditButton="True" ShowDeleteButton="True" />`). El `GridView` control ahora es similar al siguiente:

    [!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample17.aspx)]

Ejecutar la página y crear una situación de conflicto como hizo antes en la página de departamentos. Ejecute la página en dos ventanas del explorador, haga clic en **editar** en la misma línea de cada ventana y realiza un cambio diferente en cada uno de ellos. Haga clic en **actualización** en una ventana y, a continuación, haga clic en **actualización** en la otra ventana. Al hacer clic en **actualización** la segunda vez, verá la página de error que es el resultado de una excepción no controlada de simultaneidad.

[![Image22](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image44.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image43.png)

Controlar este error de una manera muy similar a cómo se administra como parte de la `ObjectDataSource` control. Abra la *Courses.aspx* página y en el `CoursesEntityDataSource` (control), especifican controladores para la `Deleted` y `Updated` eventos. La etiqueta de apertura del control ahora es similar al siguiente:

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample18.aspx)]

Antes de la `CoursesGridView` de control, agregue las siguientes `ValidationSummary` control:

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample19.aspx)]

En *Courses.aspx.cs*, agregue un `using` instrucción para el `System.Data` espacio de nombres, agregue un método que comprueba las excepciones de simultaneidad y agregar controladores para la `EntityDataSource` del control `Updated` y `Deleted`controladores. El código tendrá un aspecto similar al siguiente:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample20.cs)]

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample21.cs)]

La única diferencia entre este código y lo que hizo el `ObjectDataSource` control es que en este caso la excepción de simultaneidad en el `Exception` propiedad del objeto de argumentos de evento en lugar de en esa excepción `InnerException` propiedad.

Ejecute la página y vuelva a crear un conflicto de simultaneidad. Esta vez verá un mensaje de error:

[![Image23](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image46.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image45.png)

Con esto finaliza la introducción para el control de los conflictos de simultaneidad. El siguiente tutorial le proporcionará instrucciones sobre cómo mejorar el rendimiento en una aplicación web que utiliza Entity Framework.

> [!div class="step-by-step"]
> [Anterior](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering.md)
> [Siguiente](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application.md)
