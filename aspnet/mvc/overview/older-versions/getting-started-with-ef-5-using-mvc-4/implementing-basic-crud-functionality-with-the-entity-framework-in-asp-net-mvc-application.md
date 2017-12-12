---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
title: "Implementación de la funcionalidad básica CRUD con Entity Framework en la aplicación de ASP.NET MVC (2 de 10) | Documentos de Microsoft"
author: tdykstra
description: "La aplicación web de ejemplo de la Universidad de Contoso muestra cómo crear aplicaciones de ASP.NET MVC 4 mediante Code First de Entity Framework 5 y Visual Studio..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: f7bace3f-b85a-47ff-b5fe-49e81441cdf9
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 425335d72643da39faee6e457d552c9faa6a1f5f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="implementing-basic-crud-functionality-with-the-entity-framework-in-aspnet-mvc-application-2-of-10"></a>Implementación de la funcionalidad básica CRUD con Entity Framework en la aplicación de ASP.NET MVC (2 de 10)
====================
Por [Tom Dykstra](https://github.com/tdykstra)

[Descargar el proyecto completado](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> La aplicación web de ejemplo de la Universidad de Contoso muestra cómo crear aplicaciones de ASP.NET MVC 4 con Code First de Entity Framework 5 y Visual Studio 2012. Para obtener información acerca de la serie de tutoriales, vea [el primer tutorial de la serie](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Puede iniciar la serie de tutoriales desde el principio o [descargar un proyecto de inicio para este capítulo](building-the-ef5-mvc4-chapter-downloads.md) y empiece aquí.
> 
> > [!NOTE] 
> > 
> > Si experimenta un problema no se puede resolver, [descargar el capítulo completado](building-the-ef5-mvc4-chapter-downloads.md) e intente reproducir el problema. Por lo general puede encontrar la solución al problema comparando el código para el código completo. Para que algunos errores comunes y cómo resolverlos, vea [errores y soluciones alternativas.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


En el tutorial anterior creó una aplicación MVC que almacena y muestra los datos con Entity Framework y SQL Server LocalDB. En este tutorial le revise y personalice el CRUD (crear, leer, actualizar y eliminar) código que el scaffolding de MVC crea automáticamente para usted en controladores y vistas.

> [!NOTE]
> Es una práctica común para implementar el modelo de repositorio con el fin de crear una capa de abstracción entre el controlador y la capa de acceso a datos. Para simplificar estos tutoriales, no implemente un repositorio hasta un tutorial posterior de esta serie.


En este tutorial, creará las páginas web siguientes:

![Student_Details_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image1.png)

![Student_Edit_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image2.png)

![Student_Create_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image3.png)

![Student_delete_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image4.png)

## <a name="creating-a-details-page"></a>Crear una página de detalles

El código con scaffolding de los alumnos `Index` página deja fuera del `Enrollments` propiedad, porque esta propiedad contiene una colección. En la `Details` página mostrará el contenido de la colección en una tabla HTML.

 En *Controllers\StudentController.cs*, el método de acción para el `Details` ver usa el `Find` método para recuperar un único `Student` entidad. 

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample1.cs)]

 El valor de clave se pasa al método como el `id` parámetro y procede de los datos de ruta en el **detalles** hipervínculo en la página de índice. 

1. Abra *Views\Student\Details.cshtml*. Cada campo se muestra mediante un `DisplayFor` auxiliar, como se muestra en el ejemplo siguiente: 

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample2.cshtml)]
2. Después de la `EnrollmentDate` campos e inmediatamente antes del cierre `fieldset` etiqueta, agregue código para mostrar una lista de las inscripciones, tal como se muestra en el ejemplo siguiente:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample3.cshtml?highlight=4-22)]

    Este código recorre las entidades en el `Enrollments` propiedad de navegación. Para cada `Enrollment` entidad en la propiedad, muestra el título del curso y el grado de. Se recupera el título del curso de la `Course` entidad que se almacena en la `Course` propiedad de navegación de la `Enrollments` entidad. Todos estos datos se recupera de la base de datos automáticamente, cuando sea necesario. (En otras palabras, que esté utilizando la carga diferida aquí. No se especificó *carga diligente* para el `Courses` propiedad de navegación, por lo que la primera vez que intenta tener acceso a esa propiedad, se envía una consulta a la base de datos para recuperar los datos. Puede leer más acerca de la carga diferida y carga diligente de la [datos relacionados de lectura](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) tutorial más adelante en esta serie.)
3. Ejecute la página seleccionando el **estudiantes** ficha y haga clic en un **detalles** vínculo de Alexander Carson. Ver la lista de cursos y sus notas para el alumno seleccionado:

    ![Student_Details_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image5.png)

## <a name="updating-the-create-page"></a>Actualizar la página Crear

1. En *Controllers\StudentController.cs*, reemplace la `HttpPost``Create` método de acción con el código siguiente para agregar una `try-catch` bloque y el [atributo de enlace de](https://msdn.microsoft.com/en-us/library/system.web.mvc.bindattribute(v=vs.108).aspx) al método con scaffolding: 

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample4.cs?highlight=4,7-8,14-20)]

    Este código agrega el `Student` entidad creada por el enlazador de modelos de ASP.NET MVC para el `Students` entidad conjunto y, a continuación, guarda los cambios en la base de datos. (*Enlazador de modelos* hace referencia a la funcionalidad de ASP.NET MVC que hace que sea más fácil trabajar con los datos enviados por un formulario; un enlazador de modelos convierte registrado formulario valores a tipos de CLR y los pasa al método de acción en parámetros. In this Case, crea una instancia del enlazador de modelos un `Student` valores de entidad para que puede utilizar la propiedad de la `Form` colección.)

    El `ValidateAntiForgeryToken` atributo ayuda a evitar [falsificación de solicitudes entre sitios](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) ataques.

<a id="overpost"></a>

    > [!WARNING]
    > Security - The `Bind` attribute is added to protect against *over-posting*. For example, suppose the `Student` entity includes a `Secret` property that you don't want this web page to update.
    > 
    > [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample5.cs?highlight=7)]
    > 
    > Even if you don't have a `Secret` field on the web page, a hacker could use a tool such as [fiddler](http://fiddler2.com/home), or write some JavaScript, to post a `Secret` form value. Without the [Bind](https://msdn.microsoft.com/en-us/library/system.web.mvc.bindattribute(v=vs.108).aspx) attribute limiting the fields that the model binder uses when it creates a `Student` instance*,* the model binder would pick up that `Secret` form value and use it to update the `Student` entity instance. Then whatever value the hacker specified for the `Secret` form field would be updated in your database. The following image shows the fiddler tool adding the `Secret` field (with the value "OverPost") to the posted form values.
    > 
    > ![](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image6.png)  
    > 
    > The value "OverPost" would then be successfully added to the `Secret` property of the inserted row, although you never intended that the web page be able to update that property.
    > 
    > It's a security best practice to use the `Include` parameter with the `Bind` attribute to *whitelist* fields. It's also possible to use the `Exclude` parameter to *blacklist* fields you want to exclude. The reason `Include` is more secure is that when you add a new property to the entity, the new field is not automatically protected by an `Exclude` list.
    > 
    > Another alternative approach, and one preferred by many, is to use only view models with model binding. The view model contains only the properties you want to bind. Once the MVC model binder has finished, you copy the view model properties to the entity instance.

    Other than the `Bind` attribute, the `try-catch` block is the only change you've made to the scaffolded code. If an exception that derives from [DataException](https://msdn.microsoft.com/en-us/library/system.data.dataexception.aspx) is caught while the changes are being saved, a generic error message is displayed. [DataException](https://msdn.microsoft.com/en-us/library/system.data.dataexception.aspx) exceptions are sometimes caused by something external to the application rather than a programming error, so the user is advised to try again. Although not implemented in this sample, a production quality application would log the exception (and non-null inner exceptions ) with a logging mechanism such as [ELMAH](https://code.google.com/p/elmah/).

    The code in *Views\Student\Create.cshtml* is similar to what you saw in *Details.cshtml*, except that `EditorFor` and `ValidationMessageFor` helpers are used for each field instead of `DisplayFor`. The following example shows the relevant code:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample6.cshtml)]

    *Create.chstml* also includes `@Html.AntiForgeryToken()`, which works with the `ValidateAntiForgeryToken` attribute in the controller to help prevent [cross-site request forgery](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) attacks.

    No changes are required in *Create.cshtml*.
2. Ejecute la página seleccionando el **estudiantes** ficha y haga clic en **crear nuevo**.

    ![Student_Create_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image7.png)

    Alguna validación de datos funciona de forma predeterminada. Escriba nombres y una fecha no válida y haga clic en **crear** para ver el mensaje de error.

    ![Students_Create_page_error_message](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image8.png)

    El siguiente código resaltado muestra la comprobación de validación del modelo.

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample7.cs?highlight=5)]

    Cambie la fecha a un valor válido, como 9/1/2005 y haga clic en **crear** para ver el nuevo alumno aparecen en la **índice** página.

    ![Students_Index_page_with_new_student](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image9.png)

## <a name="updating-the-edit-post-page"></a>Actualizar la página de entrada de edición

En *Controllers\StudentController.cs*, el `HttpGet` `Edit` método (uno sin el `HttpPost` atributo) usa el `Find` método para recuperar el seleccionado `Student` entidad, como se vio en el `Details` método. No es necesario cambiar este método.

Sin embargo, reemplace la `HttpPost` `Edit` método de acción con el código siguiente para agregar una `try-catch` bloque y el [atributo de enlace de](https://msdn.microsoft.com/en-us/library/system.web.mvc.bindattribute(v=vs.108).aspx):

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample8.cs)]

Este código es similar a lo que ha visto en la `HttpPost` `Create` método. Sin embargo, en lugar de agregar la entidad creada por el enlazador de modelos para el conjunto de entidades, este código establece una marca en la entidad que indica que se ha cambiado. Cuando el [SaveChanges](https://msdn.microsoft.com/en-us/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) método se llama, el [Modified](https://msdn.microsoft.com/en-us/library/system.data.entitystate.aspx) marca hace que Entity Framework crear instrucciones SQL para actualizar la fila de la base de datos. Se actualizarán todas las columnas de la fila de la base de datos, los que el usuario no ha cambiado, incluidos y se omiten los conflictos de simultaneidad. (Aprenderá cómo controlar la simultaneidad en un tutorial posterior de esta serie).

### <a name="entity-states-and-the-attach-and-savechanges-methods"></a>Estados de entidad y las adjuntar y los métodos de SaveChanges

El contexto de base de datos realiza un seguimiento de si las entidades en memoria están sincronizadas con sus filas correspondientes en la base de datos, y esta información determina lo que ocurre cuando se llama a la `SaveChanges` método. Por ejemplo, cuando se pasa una nueva entidad a la [agregar](https://msdn.microsoft.com/en-us/library/system.data.entity.dbset.add(v=vs.103).aspx) método, que el estado de la entidad se establece en `Added`. A continuación, cuando se llama a la [SaveChanges](https://msdn.microsoft.com/en-us/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) método, el contexto de base de datos emite un SQL `INSERT` comando.

Una entidad puede estar en uno de los[después estados](https://msdn.microsoft.com/en-us/library/system.data.entitystate.aspx):

- `Added`. La entidad no existe todavía en la base de datos. El `SaveChanges` método debe emitir una `INSERT` instrucción.
- `Unchanged`. Nada debe hacerse con esta entidad mediante el `SaveChanges` método. Al leer una entidad de la base de datos, la entidad empieza con este estado.
- `Modified`. Se han modificado algunos o todos los valores de propiedad de la entidad. El `SaveChanges` método debe emitir una `UPDATE` instrucción.
- `Deleted`. La entidad se ha marcado para su eliminación. El `SaveChanges` método debe emitir un `DELETE` instrucción.
- `Detached`. La entidad no sometida a seguimiento por el contexto de base de datos.

En una aplicación de escritorio, normalmente se establecen automáticamente los cambios de estado. En un tipo de aplicación de escritorio leer una entidad y realizar cambios en algunas de sus valores de propiedad. Esto hace que su estado de entidad que se cambie automáticamente al `Modified`. A continuación, cuando se llama a `SaveChanges`, Entity Framework genera una instancia de SQL `UPDATE` instrucción que actualiza solo las propiedades reales que se cambió.

La naturaleza desconectada de las aplicaciones web no permite para esta secuencia continua. El [DbContext](https://msdn.microsoft.com/en-us/library/system.data.entity.dbcontext(v=VS.103).aspx) que lee una entidad se elimina una vez que se representa una página. Cuando el `HttpPost` `Edit` se llama al método de acción, se realiza una solicitud nueva y tiene una nueva instancia de la [DbContext](https://msdn.microsoft.com/en-us/library/system.data.entity.dbcontext(v=VS.103).aspx), por lo que tendrá que configurar manualmente el estado de la entidad `Modified.` , a continuación, cuando se llama a `SaveChanges`, Entity Framework actualiza todas las columnas de la fila de la base de datos, porque el contexto no tiene ninguna manera de saber qué propiedades ha cambiado.

Si desea que la instrucción SQL `Update` instrucción para actualizar solo los campos que el usuario cambia realmente, puede guardar los valores originales de alguna manera (por ejemplo, los campos ocultos) para que estén disponibles cuando el `HttpPost` `Edit` se llama al método. Después, podrá crear un `Student` entidad con los valores originales, llamada la `Attach` método con esa versión original de la entidad, actualizar los valores de la entidad a los nuevos valores y, a continuación, llame a `SaveChanges.` para obtener más información, consulte [ Estados de entidad y SaveChanges](https://msdn.microsoft.com/en-us/data/jj592676) y [datos locales](https://msdn.microsoft.com/en-us/data/jj592872) en el centro de desarrollo de datos de MSDN.

El código en *Views\Student\Edit.cshtml* es similar a lo que ha visto en *Create.cshtml*, y se requiere ningún cambio.

Ejecute la página seleccionando el **estudiantes** ficha y, a continuación, haga clic en un **editar** hipervínculo.

![Student_Edit_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image10.png)

Cambiar algunos de los datos y haga clic en **guardar**. Vea los datos modificados en la página de índice.

![Students_Index_page_after_edit](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image11.png)

## <a name="updating-the-delete-page"></a>Actualizar la página de borrado

En *Controllers\StudentController.cs*, el código de plantilla para la `HttpGet` `Delete` método usa la `Find` método para recuperar el seleccionado `Student` entidad, como se vio en el `Details` y `Edit` métodos. Sin embargo, para implementar un mensaje de error personalizado cuando la llamada a `SaveChanges` se produce un error, agregará algunas funciones a este método y su vista correspondiente.

Como hemos visto de actualización y crea las operaciones, las operaciones de eliminación requieren dos métodos de acción. El método que se llama en respuesta a una solicitud GET muestra una vista que proporciona al usuario una oportunidad para aprobar o cancelar la operación de eliminación. Si el usuario lo aprueba, se crea una solicitud POST. Cuando esto ocurre, el `HttpPost` `Delete` se llama al método y, a continuación, este método realiza la operación de eliminación.

Agregará un `try-catch` bloquear a la `HttpPost` `Delete` método para controlar los errores que pueden producirse cuando se actualiza la base de datos. Si se produce un error, el `HttpPost` `Delete` llamadas al método el `HttpGet` `Delete` método, pasando un parámetro que indica que se ha producido un error. El `HttpGet Delete` método, a continuación, vuelve a mostrar la página de confirmación junto con el mensaje de error, dando al usuario la oportunidad de cancelar o vuelva a intentarlo.

1. Reemplace el `HttpGet` `Delete` método de acción con el código siguiente, que administra el informe de errores: 

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample9.cs)]

    Este código acepta un [opcional](https://msdn.microsoft.com/en-us/library/dd264739.aspx) parámetro booleano que indica si se ha llamado después de un error al guardar los cambios. Este parámetro es `false` cuando el `HttpGet` `Delete` método se llama sin un error anterior. Cuando se llama la `HttpPost` `Delete` método en respuesta a un error de actualización de base de datos, el parámetro es `true` y un mensaje de error se pasa a la vista.
- Reemplace el `HttpPost` `Delete` método de acción (denominado `DeleteConfirmed`) con el código siguiente, que realiza la operación de eliminación real y captura los errores de actualización de base de datos.

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample10.cs)]

    Este código recupera la entidad seleccionada, a continuación, llama el [quitar](https://msdn.microsoft.com/en-us/library/system.data.entity.dbset.remove(v=vs.103).aspx) método para establecer el estado de la entidad como `Deleted`. Cuando `SaveChanges` se llama, una instancia de SQL `DELETE` se genere un comando. También ha cambiado el nombre del método de acción de `DeleteConfirmed` a `Delete`. El código con scaffolding denominado el `HttpPost` `Delete` método `DeleteConfirmed` para dar el `HttpPost` método una firma única. (El CLR requiere métodos sobrecargados para que tenga parámetros de método diferente). Ahora que las firmas son únicas, puede pegar con la convención MVC y usar el mismo nombre para el `HttpPost` y `HttpGet` eliminar métodos.

    Si mejora el rendimiento en una aplicación de gran volumen es una prioridad, puede evitar una consulta SQL innecesaria para recuperar la fila reemplazando las líneas de código que llame a la `Find` y `Remove` métodos con el siguiente código como se muestra en amarillo Resaltar:

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample11.cs)]

    Este código crea un `Student` entidad usando solo el valor de clave principal y, a continuación, Establece el estado de entidad en `Deleted`. Eso es todo lo que necesita de Entity Framework para eliminar la entidad.

    Como se indicó, el `HttpGet` `Delete` método no elimina los datos. Realizar una operación delete en respuesta a una operación GET de solicitud (o con este propósito, realizar cualquier operación de edición, cree la operación, o cualquier otra operación que cambia los datos) crea un riesgo de seguridad. Para obtener más información, consulte [ASP.NET MVC Sugerencia nº 46: no use eliminar vínculos porque crean vulnerabilidades de seguridad](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx) en el blog de Stephen Walther.
- En *Views\Student\Delete.cshtml*, agregar un mensaje de error entre la `h2` encabezado y el `h3` encabezado, tal como se muestra en el ejemplo siguiente:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample12.cshtml?highlight=2)]

    Ejecute la página seleccionando el **estudiantes** ficha y haga clic en un **eliminar** hipervínculo:

    ![Student_Delete_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image12.png)
- Haga clic en **eliminar**. Se abrirá la página de índice sin los estudiantes eliminado. (Verá un ejemplo del código de control de la acción de error del [control de simultaneidad](../../getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md) tutorial más adelante en esta serie.)

## <a name="ensuring-that-database-connections-are-not-left-open"></a>Asegurarse de que las conexiones de base de datos no se dejan abiertas

Para asegurarse de que se cierran correctamente las conexiones de base de datos y los recursos que mantengan liberados la, debería ver a lo que se elimina la instancia de contexto. ¿Por qué el código con scaffolding proporciona un [Dispose](https://msdn.microsoft.com/en-us/library/system.idisposable.dispose(v=vs.110).aspx) método al final de la `StudentController` clase *StudentController.cs*, tal y como se muestra en el ejemplo siguiente:

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample13.cs)]

La base de `Controller` ya la clase implementa la `IDisposable` interfaz, por lo que este código simplemente agrega una invalidación para el `Dispose(bool)` método para eliminar explícitamente la instancia de contexto.

## <a name="summary"></a>Resumen

Ahora tiene un conjunto completo de páginas que realizan operaciones CRUD sencillas para `Student` entidades. Aplicaciones auxiliares MVC se utilizan para generar elementos de interfaz de usuario para los campos de datos. Para obtener más información acerca de aplicaciones auxiliares MVC, vea [representar un formulario utilizando aplicaciones auxiliares HTML](https://msdn.microsoft.com/en-us/library/dd410596(v=VS.98).aspx) (la página es para MVC 3, pero sigue siendo pertinente para MVC 4).

En el siguiente tutorial se podrá expandir la funcionalidad de la página de índice mediante la adición de ordenación y paginación.

Vínculos a otros recursos de Entity Framework pueden encontrarse en el [mapa de contenido de acceso de datos de ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).

>[!div class="step-by-step"]
[Anterior](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)
[Siguiente](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)
