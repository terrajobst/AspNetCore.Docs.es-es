---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
title: "Implementar funcionalidad CRUD básicas con Entity Framework en la aplicación de ASP.NET MVC | Documentos de Microsoft"
author: tdykstra
description: "La aplicación web de ejemplo de la Universidad de Contoso muestra cómo crear aplicaciones de ASP.NET MVC 5 con Code First de Entity Framework 6 y Visual Studio..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/09/2015
ms.topic: article
ms.assetid: a2f70ba4-83d1-4002-9255-24732726c4f2
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: c63b8f591023b68720c523d1c9184a527a34e9cc
ms.sourcegitcommit: e4fb6b13be56a0fb2f2778623740a047d6489227
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/16/2017
---
<a name="implementing-basic-crud-functionality-with-the-entity-framework-in-aspnet-mvc-application"></a>Implementar funcionalidad CRUD básicas con Entity Framework en la aplicación de ASP.NET MVC
====================
Por [Tom Dykstra](https://github.com/tdykstra)

[Descargar el proyecto completado](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) o [descarga de PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> La aplicación web de ejemplo de la Universidad de Contoso muestra cómo crear aplicaciones de ASP.NET MVC 5 con Code First de Entity Framework 6 y Visual Studio 2013. Para obtener información acerca de la serie de tutoriales, vea [el primer tutorial de la serie](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


En el tutorial anterior creó una aplicación MVC que almacena y muestra los datos con Entity Framework y SQL Server LocalDB. En este tutorial le revise y personalice el CRUD (crear, leer, actualizar y eliminar) código que el scaffolding de MVC crea automáticamente para usted en controladores y vistas.

> [!NOTE]
> Es una práctica común para implementar el modelo de repositorio con el fin de crear una capa de abstracción entre el controlador y la capa de acceso a datos. Para mantener estos tutoriales sencilla y se centra en enseña a usar el marco de trabajo de entidad, no usan repositorios. Para obtener información sobre cómo implementar repositorios, consulte el [mapa de contenido de acceso de datos de ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).


En este tutorial, creará las páginas web siguientes:

![Student_Details_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image1.png)

![Student_Edit_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image2.png)

![Student_delete_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image3.png)

## <a name="create-a-details-page"></a>Crear una página de detalles

El código con scaffolding de los alumnos `Index` página deja fuera del `Enrollments` propiedad, porque esta propiedad contiene una colección. En la `Details` página mostrará el contenido de la colección en una tabla HTML.

 En *Controllers\StudentController.cs*, el método de acción para el `Details` ver usa el [buscar](https://msdn.microsoft.com/en-us/library/gg696418(v=VS.103).aspx) método para recuperar un único `Student` entidad. 

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample1.cs)]

El valor de clave se pasa al método como el `id` parámetro y procede *enrutar datos* en el **detalles** hipervínculo en la página de índice.

### <a name="tip-route-data"></a>Sugerencia: **enrutar datos**

Datos de ruta son datos que el enlazador de modelos que se encuentra en un segmento de dirección URL especificado en la tabla de enrutamiento. Por ejemplo, se especifica la ruta predeterminada `controller`, `action`, y `id` segmentos:

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample2.cs?highlight=3)]

En la siguiente dirección URL, se asigna la ruta predeterminada `Instructor` como el `controller`, `Index` como el `action` y 1 como el `id`; éstos son valores de datos de ruta.

`http://localhost:1230/Instructor/Index/1?courseID=2021`

"? courseID = 2021" es un valor de cadena de consulta. El enlazador de modelos también funcionará si se pasa el `id` como un valor de cadena de consulta:

`http://localhost:1230/Instructor/Index?id=1&CourseID=2021`

Las direcciones URL se crean por `ActionLink` las instrucciones en la vista de Razor. En el código siguiente, la `id` parámetro coincide con la ruta predeterminada, por lo que `id` se agrega a los datos de ruta.

[!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample3.cshtml)]

En el código siguiente, `courseID` no coincide con un parámetro en la ruta predeterminada, por lo que se agrega como una cadena de consulta.

[!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample4.cshtml)]


1. Abra *Views\Student\Details.cshtml*. Cada campo se muestra mediante un `DisplayFor` auxiliar, como se muestra en el ejemplo siguiente:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample5.cshtml)]
2. Después de la `EnrollmentDate` campos e inmediatamente antes del cierre `</dl>` etiqueta, agregue el código resaltado para mostrar una lista de las inscripciones, tal como se muestra en el ejemplo siguiente:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample6.cshtml?highlight=8-29)]

    Si no es correcta la sangría del código después de pegar el código, presione CTRL-K-D para corregirlo.

    Este código recorre las entidades en el `Enrollments` propiedad de navegación. Para cada `Enrollment` entidad en la propiedad, muestra el título del curso y el grado de. Se recupera el título del curso de la `Course` entidad que se almacena en la `Course` propiedad de navegación de la `Enrollments` entidad. Todos estos datos se recupera de la base de datos automáticamente, cuando sea necesario. (En otras palabras, que esté utilizando la carga diferida aquí. No se especificó *carga diligente* para el `Courses` propiedad de navegación, por lo que no se recuperaron las inscripciones en la misma consulta que se obtuvo de los alumnos. En su lugar, la primera vez que intenta obtener acceso a la `Enrollments` propiedad de navegación, una nueva consulta se envía a la base de datos para recuperar los datos. Puede leer más acerca de la carga diferida y carga diligente de la [datos relacionados de lectura](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) tutorial más adelante en esta serie.)
3. Ejecute la página seleccionando el **estudiantes** ficha y haga clic en un **detalles** vínculo de Alexander Carson. (Si presiona CTRL + F5 mientras está abierto el archivo Details.cshtml, obtendrá un error HTTP 400 porque Visual Studio intenta ejecutar la página de detalles, pero no se ha alcanzado desde un vínculo que especifica los estudiantes para mostrar. In that Case, simplemente quitar "Student/detalles" de la dirección URL y vuelva a intentarlo, o cierre el explorador, haga clic en el proyecto y haga clic en **vista**y, a continuación, haga clic en **ver en el explorador**.)

    Ver la lista de cursos y sus notas para el alumno seleccionado:

    ![Student_Details_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image4.png)

## <a name="update-the-create-page"></a>Actualizar la página Crear

1. En *Controllers\StudentController.cs*, reemplace la `HttpPost``Create` método de acción con el código siguiente para agregar una `try-catch` bloquear y quitar `ID` desde el [atributo de enlace](https://msdn.microsoft.com/en-us/library/system.web.mvc.bindattribute(v=vs.108).aspx) para el método con scaffolding:

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample7.cs?highlight=3,5-6,13-18)]

    Este código agrega el `Student` entidad creada por el enlazador de modelos de ASP.NET MVC para el `Students` entidad conjunto y, a continuación, guarda los cambios en la base de datos. (*Enlazador de modelos* hace referencia a la funcionalidad de ASP.NET MVC que hace que sea más fácil trabajar con los datos enviados por un formulario; un enlazador de modelos convierte registrado formulario valores a tipos de CLR y los pasa al método de acción en parámetros. In this Case, crea una instancia del enlazador de modelos un `Student` valores de entidad para que puede utilizar la propiedad de la `Form` colección.)

    Que se han suprimido `ID` desde el enlace de atributo porque `ID` es el valor de clave principal que SQL Server establecerá automáticamente cuando se inserta la fila. Entrada del usuario no establece la `ID` valor.

    <a id="overpost"></a>

    ### <a name="security-warning---the-validateantiforgerytoken-attribute-helps-prevent-cross-site-request-forgerysecurityxsrfcsrf-prevention-in-aspnet-mvc-and-web-pagesmd-attacks-it-requires-a-corresponding-htmlantiforgerytoken-statement-in-the-view-which-youll-see-later"></a>Advertencia de seguridad: el `ValidateAntiForgeryToken` atributo ayuda a evitar [falsificación de solicitudes entre sitios](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) ataques. Requiere su correspondiente `Html.AntiForgeryToken()` instrucción en la vista, que verá más adelante.

    El `Bind` atributo es una manera de proteger contra *exceso contabilización* en crear escenarios. Por ejemplo, suponga que la `Student` entidad incluye un `Secret` propiedad que no desea que esta página web para establecer.

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample8.cs?highlight=7)]

    Incluso si no tienes un `Secret` campo en la página web, un pirata informático podría utilizar una herramienta como [fiddler](http://fiddler2.com/home), o escribir código de JavaScript, para registrar un `Secret` formar el valor. Sin el [enlazar](https://msdn.microsoft.com/en-us/library/system.web.mvc.bindattribute(v=vs.108).aspx) atributo limitar los campos que el enlazador de modelos que se utiliza cuando crea un `Student` instancia*,* el enlazador de modelos seleccionará que `Secret` formato de valor y utilizarlo para crear el `Student` instancia de entidad. A continuación, cualquier valor el pirata especificado para el `Secret` campo de formulario se actualizarán en la base de datos. La siguiente imagen muestra la fiddler herramienta agregando el `Secret` campo (con el valor "OverPost") a los valores de formulario expuesto.

    ![](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image5.png)  

    El valor "OverPost", a continuación, se agregaría correctamente a la `Secret` propiedad de las filas insertadas de fila, aunque se habían previsto que la página web pueda establece esta propiedad.

    Es un procedimiento recomendado de seguridad para usar el `Include` parámetro con el `Bind` atribuir a *lista blanca de direcciones* campos. También es posible utilizar el `Exclude` parámetro *lista negra* campos que desee excluir. El motivo `Include` es más seguro es que cuando se agrega una nueva propiedad a la entidad, el nuevo campo no está automáticamente protegido por un `Exclude` lista.

    Puede evitar que overposting en escenarios de edición es leer primero la entidad desde la base de datos y, a continuación, llamando a `TryUpdateModel`, pasando una lista de propiedades permitidas explícita. Que es el método utilizado en estos tutoriales.

    Es una manera alternativa para evitar que overposting que muchos desarrolladores prefieren usar modelos de vista en lugar de las clases de entidad con el enlace de modelos. Incluir solo las propiedades que desea actualizar en el modelo de vista. Una vez que haya finalizado el enlazador de modelos MVC, copie las propiedades del modelo de vista a la instancia de entidad, opcionalmente mediante una herramienta como [AutoMapper](http://automapper.org/). Usar base de datos. Entrada en la instancia de entidad para establecer su estado a sin cambios y, a continuación, Property("PropertyName"). IsModified en true en cada propiedad de entidad que se incluye en el modelo de vista. Este método da tanto en Editar y crear escenarios.

    Distinto de la `Bind` atributo, el `try-catch` bloque es el único cambio que haya realizado en el código con scaffolding. Si una excepción que se deriva de [DataException](https://msdn.microsoft.com/en-us/library/system.data.dataexception.aspx) es detecta mientras se guardan los cambios, se muestra un mensaje de error genérico. [DataException](https://msdn.microsoft.com/en-us/library/system.data.dataexception.aspx) excepciones a veces se deben a algo externo a la aplicación en lugar de un error de programación, por lo que el usuario se recomienda que vuelva a intentarlo. Aunque no ha implementado en este ejemplo, una aplicación de calidad de producción debería registrar la excepción. Para obtener más información, consulte el **registro para obtener información** sección [supervisión y telemetría (creación reales en la nube de aplicaciones con Azure)](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry.md#log).

    El código en *Views\Student\Create.cshtml* es similar a lo que ha visto en *Details.cshtml*, salvo que `EditorFor` y `ValidationMessageFor` se usan aplicaciones auxiliares para cada campo en lugar de `DisplayFor`. Este es el código relevante:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample9.cshtml)]

    *Create.chstml* también incluye `@Html.AntiForgeryToken()`, que funciona con la `ValidateAntiForgeryToken` atributo en el controlador para ayudar a evitar [falsificación de solicitudes entre sitios](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) ataques.

    Se requiere ningún cambio en *Create.cshtml*.
2. Ejecute la página seleccionando el **estudiantes** ficha y haga clic en **crear nuevo**.
3. Escriba nombres y una fecha no válida y haga clic en **crear** para ver el mensaje de error.

    ![Students_Create_page_error_message](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image6.png)

    Se trata de validación del lado servidor que obtendrá de forma predeterminada; en un tutorial posterior verá cómo agregar atributos que se generarán código para la validación del lado cliente también. El siguiente código resaltado muestra la comprobación de validación del modelo en el **Create** método.

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample10.cs?highlight=1)]
4. Cambie la fecha a un valor válido y haga clic en **crear** para ver el nuevo alumno aparecen en la **índice** página.

    ![Students_Index_page_with_new_student](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image7.png)

## <a name="update-the-edit-httppost-method"></a>Actualizar el método de HttpPost Edit

En *Controllers\StudentController.cs*, el `HttpGet` `Edit` método (uno sin el `HttpPost` atributo) usa el `Find` método para recuperar el seleccionado `Student` entidad, como se vio en el `Details` método. No es necesario cambiar este método.

Sin embargo, reemplace la `HttpPost` `Edit` método de acción con el código siguiente:

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample11.cs)]

Estos cambios implementan un procedimiento recomendado de seguridad para evitar [de publicación excesiva](#overpost), la scaffolder genera un `Bind` de atributo y agrega la entidad creada por el enlazador de modelos para el conjunto con una marca de modificado de entidades. Que el código ya no se recomienda porque la `Bind` atributo borra los datos previamente existentes en los campos no aparecen en la `Include` parámetro. En el futuro, se actualizará el scaffolder de controlador MVC para que no genera `Bind` atributos para los métodos de edición.

El nuevo código lee la entidad existente y llama [TryUpdateModel](https://msdn.microsoft.com/en-us/library/system.web.mvc.controller.tryupdatemodel(v=vs.118).aspx) para actualizar los campos de entrada del usuario en los datos de formulario expuesto. Seguimiento de conjuntos de cambios automático de Entity Framework la [Modified](https://msdn.microsoft.com/en-us/library/system.data.entitystate.aspx) marca en la entidad. Cuando el [SaveChanges](https://msdn.microsoft.com/en-us/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) método se llama, el `Modified` marca hace que Entity Framework crear instrucciones SQL para actualizar la fila de la base de datos. [Conflictos de simultaneidad](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md) se omiten, y se actualizan todas las columnas de la fila de la base de datos, los que el usuario no ha cambiado incluidos. (Un tutorial posterior muestra cómo controlar los conflictos de simultaneidad, y si solo desea campos individuales que se actualizará en la base de datos, puede establecer la entidad en Unchanged y establecer individuales campos a Modified.)

Como práctica recomendada para evitar overposting, los campos que desea que se pueda actualizar la página de edición están en la lista blanca de la `TryUpdateModel` parámetros. Actualmente no hay ningún campo adicional que va a proteger, pero mostrar los campos que desea que el enlazador de modelos para enlazar garantiza que si agrega campos para el modelo de datos en el futuro, que están automáticamente protegidos hasta que se agreguen de forma explícita aquí.

Como resultado de estos cambios, la firma del método del método editar HttpPost es el mismo que el método de edición HttpGet; por lo tanto, ha cambiado el nombre del método EditPost.

> [!TIP] 
> 
> **Estados de entidad y las adjuntar y los métodos de SaveChanges**
> 
> El contexto de base de datos realiza un seguimiento de si las entidades en memoria están sincronizadas con sus filas correspondientes en la base de datos, y esta información determina lo que ocurre cuando se llama a la `SaveChanges` método. Por ejemplo, cuando se pasa una nueva entidad a la [agregar](https://msdn.microsoft.com/en-us/library/system.data.entity.dbset.add(v=vs.103).aspx) método, que el estado de la entidad se establece en `Added`. A continuación, cuando se llama a la [SaveChanges](https://msdn.microsoft.com/en-us/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) método, el contexto de base de datos emite un SQL `INSERT` comando.
> 
> Una entidad puede estar en uno de los[después estados](https://msdn.microsoft.com/en-us/library/system.data.entitystate.aspx):
> 
> - `Added`. La entidad no existe todavía en la base de datos. El `SaveChanges` método debe emitir una `INSERT` instrucción.
> - `Unchanged`. Nada debe hacerse con esta entidad mediante el `SaveChanges` método. Al leer una entidad de la base de datos, la entidad empieza con este estado.
> - `Modified`. Se han modificado algunos o todos los valores de propiedad de la entidad. El `SaveChanges` método debe emitir una `UPDATE` instrucción.
> - `Deleted`. La entidad se ha marcado para su eliminación. El `SaveChanges` método debe emitir un `DELETE` instrucción.
> - `Detached`. La entidad no sometida a seguimiento por el contexto de base de datos.
> 
> En una aplicación de escritorio, normalmente se establecen automáticamente los cambios de estado. En un tipo de aplicación de escritorio leer una entidad y realizar cambios en algunas de sus valores de propiedad. Esto hace que su estado de entidad que se cambie automáticamente al `Modified`. A continuación, cuando se llama a `SaveChanges`, Entity Framework genera una instancia de SQL `UPDATE` instrucción que actualiza solo las propiedades reales que se cambió.
> 
> La naturaleza desconectada de las aplicaciones web no permite para esta secuencia continua. El [DbContext](https://msdn.microsoft.com/en-us/library/system.data.entity.dbcontext(v=VS.103).aspx) que lee una entidad se elimina una vez que se representa una página. Cuando el `HttpPost` `Edit` se llama al método de acción, se realiza una solicitud nueva y tiene una nueva instancia de la [DbContext](https://msdn.microsoft.com/en-us/library/system.data.entity.dbcontext(v=VS.103).aspx), por lo que tendrá que configurar manualmente el estado de la entidad `Modified.` , a continuación, cuando se llama a `SaveChanges`, Entity Framework actualiza todas las columnas de la fila de la base de datos, porque el contexto no tiene ninguna manera de saber qué propiedades ha cambiado.
> 
> Si desea que la instrucción SQL `Update` instrucción para actualizar solo los campos que el usuario cambia realmente, puede guardar los valores originales de alguna manera (por ejemplo, los campos ocultos) para que estén disponibles cuando el `HttpPost` `Edit` se llama al método. Después, podrá crear un `Student` entidad con los valores originales, llamada la `Attach` método con esa versión original de la entidad, actualizar los valores de la entidad a los nuevos valores y, a continuación, llame a `SaveChanges.` para obtener más información, consulte [ Estados de entidad y SaveChanges](https://msdn.microsoft.com/en-us/data/jj592676) y [datos locales](https://msdn.microsoft.com/en-us/data/jj592872) en el centro de desarrollo de datos de MSDN.


El código HTML y Razor en *Views\Student\Edit.cshtml* es similar a lo que ha visto en *Create.cshtml*, y se requiere ningún cambio.

Ejecute la página seleccionando el **estudiantes** ficha y, a continuación, haga clic en un **editar** hipervínculo.

![Student_Edit_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image8.png)

Cambiar algunos de los datos y haga clic en **guardar**. Vea los datos modificados en la página de índice.

![Students_Index_page_after_edit](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image9.png)

## <a name="updating-the-delete-page"></a>Actualizar la página de borrado

En *Controllers\StudentController.cs*, el código de plantilla para la `HttpGet` `Delete` método usa la `Find` método para recuperar el seleccionado `Student` entidad, como se vio en el `Details` y `Edit` métodos. Sin embargo, para implementar un mensaje de error personalizado cuando la llamada a `SaveChanges` se produce un error, agregará algunas funciones a este método y su vista correspondiente.

Como hemos visto de actualización y crea las operaciones, las operaciones de eliminación requieren dos métodos de acción. El método que se llama en respuesta a una solicitud GET muestra una vista que proporciona al usuario una oportunidad para aprobar o cancelar la operación de eliminación. Si el usuario lo aprueba, se crea una solicitud POST. Cuando esto ocurre, el `HttpPost` `Delete` se llama al método y, a continuación, este método realiza la operación de eliminación.

Agregará un `try-catch` bloquear a la `HttpPost` `Delete` método para controlar los errores que pueden producirse cuando se actualiza la base de datos. Si se produce un error, el `HttpPost` `Delete` llamadas al método el `HttpGet` `Delete` método, pasando un parámetro que indica que se ha producido un error. El `HttpGet Delete` método, a continuación, vuelve a mostrar la página de confirmación junto con el mensaje de error, dando al usuario la oportunidad de cancelar o vuelva a intentarlo.

1. Reemplace el `HttpGet` `Delete` método de acción con el código siguiente, que administra el informe de errores: 

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample12.cs?highlight=1,7-10)]

    Este código acepta un [parámetro opcional](https://msdn.microsoft.com/en-us/library/dd264739.aspx) que indica si se llamó al método después de un error al guardar los cambios. Este parámetro es `false` cuando el `HttpGet` `Delete` método se llama sin un error anterior. Cuando se llama la `HttpPost` `Delete` método en respuesta a un error de actualización de base de datos, el parámetro es `true` y un mensaje de error se pasa a la vista.
- Reemplace el `HttpPost` `Delete` método de acción (denominado `DeleteConfirmed`) con el código siguiente, que realiza la operación de eliminación real y captura los errores de actualización de base de datos.

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample13.cs)]

    Este código recupera la entidad seleccionada, a continuación, llama el [quitar](https://msdn.microsoft.com/en-us/library/system.data.entity.dbset.remove(v=vs.103).aspx) método para establecer el estado de la entidad como `Deleted`. Cuando `SaveChanges` se llama, una instancia de SQL `DELETE` se genere un comando. También ha cambiado el nombre del método de acción de `DeleteConfirmed` a `Delete`. El código con scaffolding denominado el `HttpPost` `Delete` método `DeleteConfirmed` para dar el `HttpPost` método una firma única. (El CLR requiere métodos sobrecargados para que tenga parámetros de método diferente). Ahora que las firmas son únicas, puede pegar con la convención MVC y usar el mismo nombre para el `HttpPost` y `HttpGet` eliminar métodos.

    Si mejora el rendimiento en una aplicación de gran volumen es una prioridad, puede evitar una consulta SQL innecesaria para recuperar la fila reemplazando las líneas de código que llame a la `Find` y `Remove` métodos con el código siguiente:

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample14.cs)]

    Este código crea un `Student` entidad usando solo el valor de clave principal y, a continuación, Establece el estado de entidad en `Deleted`. Eso es todo lo que necesita de Entity Framework para eliminar la entidad.

    Como se indicó, el `HttpGet` `Delete` método no elimina los datos. Realizar una operación delete en respuesta a una operación GET de solicitud (o con este propósito, realizar cualquier operación de edición, cree la operación, o cualquier otra operación que cambia los datos) crea un riesgo de seguridad. Para obtener más información, consulte [ASP.NET MVC Sugerencia nº 46: no use eliminar vínculos porque crean vulnerabilidades de seguridad](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx) en el blog de Stephen Walther.
- En *Views\Student\Delete.cshtml*, agregar un mensaje de error entre la `h2` encabezado y el `h3` encabezado, tal como se muestra en el ejemplo siguiente:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample15.cshtml?highlight=2)]

    Ejecute la página seleccionando el **estudiantes** ficha y haga clic en un **eliminar** hipervínculo:

    ![Student_Delete_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image10.png)
- Haga clic en **eliminar**. Se abrirá la página de índice sin los estudiantes eliminado. (Verá un ejemplo del código de control de la acción de error del [tutorial de simultaneidad](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md).)

## <a name="closing-database-connections"></a>Cierre las conexiones de base de datos

Para cerrar las conexiones de base de datos y liberar los recursos que contienen tan pronto como sea posible, elimine la instancia del contexto cuando haya terminado con él. ¿Por qué el código con scaffolding proporciona un [Dispose](https://msdn.microsoft.com/en-us/library/system.idisposable.dispose(v=vs.110).aspx) método al final de la `StudentController` clase *StudentController.cs*, tal y como se muestra en el ejemplo siguiente:

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample16.cs)]

La base de `Controller` ya la clase implementa la `IDisposable` interfaz, por lo que este código simplemente agrega una invalidación para el `Dispose(bool)` método para eliminar explícitamente la instancia de contexto.

<a id="transactions"></a>
## <a name="handling-transactions"></a>Controlar transacciones

De forma predeterminada el Entity Framework implementa de manera implícita las transacciones. En escenarios donde realizar cambios en varias filas o tablas y, a continuación, llamar a `SaveChanges`, Entity Framework automáticamente se asegura que todos los cambios se realizan correctamente o producirá un error en todas. Si se realizan algunos cambios en primer lugar y, a continuación, se produce un error, los cambios se deshacen automáticamente. Para escenarios donde necesita más control, por ejemplo, si van a incluir operaciones realizadas fuera de Entity Framework en una transacción, consulte [trabajar con transacciones](https://msdn.microsoft.com/en-US/data/dn456843) en MSDN.

## <a name="summary"></a>Resumen

Ahora tiene un conjunto completo de páginas que realizan operaciones CRUD sencillas para `Student` entidades. Aplicaciones auxiliares MVC se utilizan para generar elementos de interfaz de usuario para los campos de datos. Para obtener más información acerca de aplicaciones auxiliares MVC, vea [representar un formulario utilizando aplicaciones auxiliares HTML](https://msdn.microsoft.com/en-us/library/dd410596(v=VS.98).aspx) (la página es para MVC 3, pero sigue siendo pertinente para MVC 5).

En el siguiente tutorial se podrá expandir la funcionalidad de la página de índice mediante la adición de ordenación y paginación.

Vota sobre cómo le gustó este tutorial y lo que podemos mejorar. También puede solicitar nuevos temas en [mostrar Me cómo con código](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).

Vínculos a otros recursos de Entity Framework se pueden encontrar en [ASP.NET Data Access: recursos recomendados](../../../../whitepapers/aspnet-data-access-content-map.md).

>[!div class="step-by-step"]
[Anterior](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)
[Siguiente](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)
