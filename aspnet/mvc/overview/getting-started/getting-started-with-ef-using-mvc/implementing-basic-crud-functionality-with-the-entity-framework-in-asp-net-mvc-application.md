---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
title: 'Tutorial: Implementar la funcionalidad CRUD con Entity Framework en ASP.NET MVC | Microsoft Docs'
description: Revisar y personalizar el crear, leer, actualizar, eliminar (CRUD) código que el scaffolding de MVC crea automáticamente en los controladores y vistas.
author: tdykstra
ms.author: riande
ms.date: 01/22/2019
ms.topic: tutorial
ms.assetid: a2f70ba4-83d1-4002-9255-24732726c4f2
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 9ed388543dd54d209ff2a0b92df4f7659962582c
ms.sourcegitcommit: c47d7c131eebbcd8811e31edda210d64cf4b9d6b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2019
ms.locfileid: "55236476"
---
# <a name="tutorial-implement-crud-functionality-with-the-entity-framework-in-aspnet-mvc"></a>Tutorial: Implementar la funcionalidad CRUD con Entity Framework en ASP.NET MVC

En el [tutorial anterior](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md), creó una aplicación de MVC que almacena y muestra los datos con el Entity Framework (EF) 6 y SQL Server LocalDB. En este tutorial, revisa y personalizar el crear, lee, actualizar, elimina (CRUD) código que el scaffolding de MVC crea automáticamente para usted en controladores y vistas.

> [!NOTE]
> Es una práctica habitual implementar el modelo de repositorio con el fin de crear una capa de abstracción entre el controlador y la capa de acceso a datos. Para que estos tutoriales sean sencillos y se centren en enseñar a usar EF 6 propio, no se usan repositorios. Para obtener información sobre cómo implementar repositorios, consulte el [mapa de contenido de acceso de datos de ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).

Estos son ejemplos de las páginas web que crea:

![Captura de pantalla de la página de detalles de estudiante.](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image1.png)

![Página de creación de una captura de pantalla del alumno.](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image2.png)

![Una captura de pantalla ot el alumno Eliminar página.](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image3.png)

En este tutorial ha:

> [!div class="checklist"]
> * Crear una página de detalles
> * Actualizar la página Create
> * Actualice el método HttpPost Edit
> * Actualizar la página Delete
> * Conexiones de cierre de la base de datos
> * Controlar transacciones

## <a name="prerequisites"></a>Requisitos previos

* [Crear el modelo de datos de Entity Framework](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)

## <a name="create-a-details-page"></a>Crear una página de detalles

El código con scaffolding para los estudiantes `Index` página deja fuera la `Enrollments` propiedad, porque contiene una colección. En el `Details` página, se mostrará el contenido de la colección en una tabla HTML.

En *Controllers\StudentController.cs*, el método de acción para el `Details` ver usa el [buscar](https://msdn.microsoft.com/library/gg696418(v=VS.103).aspx) método para recuperar una única `Student` entidad.

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample1.cs)]

El valor de clave se pasa al método como el `id` parámetro y procede de *enrutar datos* en el **detalles** hipervínculo en la página de índice.

### <a name="tip-route-data"></a>Sugerencia: **Datos de ruta**

Datos de ruta son datos que el enlazador de modelos se encuentra en un segmento de dirección URL especificado en la tabla de enrutamiento. Por ejemplo, la ruta predeterminada especifica `controller`, `action`, y `id` segmentos:

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample2.cs?highlight=3)]

En la siguiente dirección URL, la ruta predeterminada asigna `Instructor` como el `controller`, `Index` como el `action` y 1 como el `id`; estos son los valores de datos de ruta.

`http://localhost:1230/Instructor/Index/1?courseID=2021`

`?courseID=2021` es un valor de cadena de consulta. El enlazador de modelos también funcionará si se pasa el `id` como un valor de cadena de consulta:

`http://localhost:1230/Instructor/Index?id=1&CourseID=2021`

Las direcciones URL se crean mediante `ActionLink` instrucciones en la vista de Razor. En el código siguiente, la `id` parámetro coincide con la ruta predeterminada, por lo que `id` se agrega a los datos de ruta.

[!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample3.cshtml)]

En el código siguiente, `courseID` no coincide con un parámetro en la ruta predeterminada, por lo que se agrega como una cadena de consulta.

[!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample4.cshtml)]

### <a name="to-create-the-details-page"></a>Para crear la página de detalles

1. Abra *Views\Student\Details.cshtml*.

   Cada campo se muestra mediante un `DisplayFor` auxiliar, como se muestra en el ejemplo siguiente:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample5.cshtml)]

2. Después de la `EnrollmentDate` campo e inmediatamente antes del cierre `</dl>` etiqueta, agregue el código resaltado para mostrar una lista de inscripciones, tal como se muestra en el ejemplo siguiente:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample6.cshtml?highlight=8-29)]

    Si la sangría de código es incorrecta después de pegar el código, presione **Ctrl**+**K**, **Ctrl**+**d.** para dar formato a él.

    Este código recorre en bucle las entidades en la propiedad de navegación `Enrollments`. Para cada `Enrollment` entidad en la propiedad, muestra el título del curso y el curso. El título del curso se recupera de la `Course` entidad que se almacena en el `Course` propiedad de navegación de la `Enrollments` entidad. Todos estos datos se recupera de la base de datos automáticamente, cuando sea necesario. En otras palabras, se usa la carga diferida aquí. No especificó *carga diligente* para el `Courses` propiedad de navegación, por lo que la inscripción no se recuperaron en la misma consulta que se ha recibido los alumnos. En su lugar, la primera vez intenta obtener acceso a la `Enrollments` propiedad de navegación, una nueva consulta se envía a la base de datos para recuperar los datos. Puede leer más acerca de la carga diferida y diligente en la [leer datos relacionados](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) tutorial más adelante en esta serie.

3. Abra la página de detalles, inicie el programa (**Ctrl**+**F5**), seleccione el **estudiantes** ficha y, a continuación, haga clic en el **detalles** vínculo de Alexander Carson. (Si presiona **Ctrl**+**F5** mientras el *Details.cshtml* archivo está abierto, obtendrá un error HTTP 400. Esto es porque Visual Studio intenta ejecutar la página de detalles, pero no se ha alcanzado desde un vínculo que especifica el estudiante para mostrar. Si esto ocurre, quitar "Student/detalles" de la dirección URL y vuelva a intentarlo, o bien, cierre el explorador, haga clic en el proyecto y haga clic en **vista** > **ver en el explorador**.)

    Consulte la lista de cursos y calificaciones para el alumno seleccionado.

4. Cierre el explorador.

## <a name="update-the-create-page"></a>Actualizar la página Create

1. En *Controllers\StudentController.cs*, reemplace el <xref:System.Web.Mvc.HttpPostAttribute> `Create` método de acción con el código siguiente. Este código agrega un `try-catch` bloque y quita `ID` desde el <xref:System.Web.Mvc.BindAttribute> atributo para el método con scaffolding:

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample7.cs?highlight=3,5-6,13-18)]

    Este código agrega el `Student` entidad creada por el enlazador de modelos ASP.NET MVC para la `Students` entidad conjunto y, a continuación, guarda los cambios en la base de datos. *Enlazador de modelos* hace referencia a la funcionalidad de ASP.NET MVC que hace que sea más fácil trabajar con los datos enviados por un formulario; un enlazador de modelos formulario registrado se convierte valores a tipos CLR y los pasa al método de acción en parámetros. En este caso, el enlazador de modelos crea un `Student` entidad automáticamente mediante la propiedad de los valores de la `Form` colección.

    Se ha quitado `ID` desde el enlace de atributo porque `ID` es el valor de clave principal que SQL Server establecerá automáticamente cuando se inserta la fila. Entrada del usuario no establece la `ID` valor.

    <a id="overpost"></a>

    ### <a name="security-warning---the-validateantiforgerytoken-attribute-helps-prevent-cross-site-request-forgerysecurityxsrfcsrf-prevention-in-aspnet-mvc-and-web-pagesmd-attacks-it-requires-a-corresponding-htmlantiforgerytoken-statement-in-the-view-which-youll-see-later"></a>Advertencia de seguridad - la `ValidateAntiForgeryToken` atributo ayuda a evitar [falsificación de solicitud entre sitios](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) ataques. Requiere un correspondiente `Html.AntiForgeryToken()` instrucción en la vista, que verá más adelante.

    El `Bind` atributo es una manera de protegerse contra *publicación excesiva* en escenarios de creación. Por ejemplo, suponga que el `Student` entidad incluye un `Secret` propiedad que no desea que esta página web para establecer.

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample8.cs?highlight=7)]

    Incluso si no tiene un `Secret` campo en la página web, un pirata informático podría usar una herramienta como [fiddler](http://fiddler2.com/home), o escribir código de JavaScript, para registrar un `Secret` valor de formulario. Sin el <xref:System.Web.Mvc.BindAttribute> atributo limitar los campos que el enlazador de modelos usa cuando crea un `Student` instancia<em>,</em> el enlazador de modelos seleccionaría ese `Secret` valor de formulario y usarlo para crear el `Student` instancia de entidad. Después, el valor que el hacker haya especificado para el campo de formulario `Secret` se actualizaría en la base de datos. La siguiente imagen muestra la fiddler herramienta agregando el `Secret` campo (con el valor "OverPost") a los valores de formulario enviados.

    ![](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image5.png)

    Después, el valor "OverPost" se agregaría correctamente a la propiedad `Secret` de la fila insertada, aunque no hubiera previsto que la página web pudiera establecer esa propiedad.

    Es mejor usar la `Include` parámetro con el `Bind` atributo *lista blanca* campos. También es posible utilizar el `Exclude` parámetro *lista de bloqueados* campos que desee excluir. El motivo `Include` es más seguro es que cuando se agrega una nueva propiedad a la entidad, el nuevo campo no está automáticamente protegido por un `Exclude` lista.

    Puede evitar la publicación excesiva en escenarios de edición es leer primero la entidad desde la base de datos y, a continuación, llamando a `TryUpdateModel`, pasando una lista explícita de propiedades permitidas. Que es el método utilizado en estos tutoriales.

    Es una manera alternativa de evitar la publicación excesiva que muchos desarrolladores prefieren usar modelos de vista en lugar de las clases de entidad con enlace de modelos. Incluya en el modelo de vista solo las propiedades que quiera actualizar. Cuando haya finalizado el enlazador de modelos MVC, copie las propiedades del modelo de vista en la instancia de entidad, opcionalmente con una herramienta como [AutoMapper](http://automapper.org/). Use la base de datos. Entrada en la instancia de entidad para establecer su estado en Unchanged y, a continuación, establezca Property("PropertyName"). IsModified en true en cada propiedad de entidad que se incluye en el modelo de vista. Este método funciona tanto en escenarios de edición como de creación.

    No sea el `Bind` atributo, el `try-catch` bloque es el único cambio que haya realizado en el código con scaffolding. Si se detecta una excepción derivada de <xref:System.Data.DataException> mientras se guardan los cambios, se muestra un mensaje de error genérico. En ocasiones, las excepciones <xref:System.Data.DataException> se deben a algo externo a la aplicación y no a un error de programación, por lo que se recomienda al usuario que vuelva a intentarlo. Aunque no se ha implementado en este ejemplo, en una aplicación de producción de calidad se debería registrar la excepción. Para obtener más información, vea la sección **Registro para obtener información** de [Supervisión y telemetría (creación de aplicaciones de nube reales con Azure)](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry.md#log).

    El código en *Views\Student\Create.cshtml* es similar a lo vimos en *Details.cshtml*, salvo que `EditorFor` y `ValidationMessageFor` se usan las aplicaciones auxiliares para cada campo en lugar de `DisplayFor`. Este es el código pertinente:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample9.cshtml)]

    *Create.cshtml* también incluye `@Html.AntiForgeryToken()`, que funciona con el `ValidateAntiForgeryToken` atributo en el controlador para ayudar a evitar [falsificación de solicitud entre sitios](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) ataques.

    Se requiere ningún cambio en *Create.cshtml*.

2. Ejecute la página, inicie el programa, seleccione el **estudiantes** ficha y, a continuación, haga clic en **crear nuevo**.

3. Escriba los nombres y una fecha no válida y haga clic en **crear** para ver el mensaje de error.

    Se trata de validación del lado servidor que obtendrá de forma predeterminada. En un tutorial posterior, verá cómo agregar atributos que se va a generar código para la validación del lado cliente. El código resaltado siguiente muestra la comprobación de validación del modelo en el **Create** método.

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample10.cs?highlight=1)]

4. Cambie la fecha por un valor válido y haga clic en **Crear** para ver el alumno nuevo en la página **Index**.

5. Cierre el explorador.

## <a name="update-httppost-edit-method"></a>Actualizar el método HttpPost Edit

1. Reemplace el <xref:System.Web.Mvc.HttpPostAttribute> `Edit` método de acción con el código siguiente:

   [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample11.cs)]

   > [!NOTE]
   > En *Controllers\StudentController.cs*, `HttpGet Edit` método (la otra sin la `HttpPost` atributo) usa el `Find` método para recuperar el texto seleccionado `Student` entidad, como se vio en el `Details`método. No es necesario cambiar este método.

   Estos cambios implementan una práctica recomendada de seguridad para evitar [publicación excesiva](#overpost), el proveedor de scaffolding generado un `Bind` atributo y agrega la entidad creada por el enlazador de modelos para el conjunto de entidades con una marca de modificado. Que el código ya no se recomienda porque el `Bind` atributo borra los datos ya existentes en los campos no aparecen en la `Include` parámetro. En el futuro, el proveedor de scaffolding de controlador MVC se actualizará para que no genere `Bind` atributos para los métodos de edición.

   El código nuevo lee la entidad existente y llama a <xref:System.Web.Mvc.Controller.TryUpdateModel%2A> para actualizar los campos de entrada del usuario en los datos de formulario enviados. Seguimiento de conjuntos de cambios automático de Entity Framework la [EntityState.Modified](<xref:System.Data.EntityState.Modified>) marca en la entidad. Cuando el [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) se invoca el <xref:System.Data.EntityState.Modified> marca hace que Entity Framework crear instrucciones SQL para actualizar la fila de la base de datos. [Los conflictos de simultaneidad](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md) se omiten, y se actualizan todas las columnas de la fila de la base de datos, los que el usuario no ha cambiado incluidos. (Un tutorial posterior muestra cómo controlar los conflictos de simultaneidad, y si sólo desea campos individuales en actualizarse en la base de datos, puede establecer la entidad en [EntityState.Unchanged](<xref:System.Data.EntityState.Unchanged>) y establezca los campos individuales en [ EntityState.Modified](<xref:System.Data.EntityState.Modified>).)

   Para evitar la publicación excesiva, los campos que desea que se puedan actualizar por la página de edición están en la lista blanca en la `TryUpdateModel` parámetros. Actualmente no se está protegiendo ningún campo adicional, pero enumerar los campos que quiere que el enlazador de modelos enlace garantiza que, si en el futuro agrega campos al modelo de datos, se protejan automáticamente hasta que los agregue aquí de forma explícita.

   Como resultado de estos cambios, la firma del método del método HttpPost Edit es el mismo que el método HttpGet edit; por lo tanto, ha cambiado el nombre del método EditPost.

   > [!TIP]
   >
   > **Estados de entidad y la asociación y métodos de SaveChanges**
   >
   > El contexto de la base de datos realiza el seguimiento de si las entidades en memoria están sincronizadas con sus filas correspondientes en la base de datos, y esta información determina lo que ocurre cuando se llama al método `SaveChanges`. Por ejemplo, al pasar una nueva entidad a la [agregar](https://msdn.microsoft.com/library/system.data.entity.dbset.add(v=vs.103).aspx) método, que se establece el estado de la entidad en `Added`. A continuación, cuando se llama a la [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) método, el contexto de base de datos emite un SQL `INSERT` comando.
   >
   > Una entidad puede estar en uno de los siguientes [estados](xref:System.Data.EntityState):
   >
   > - `Added`. La entidad no existe todavía en la base de datos. El `SaveChanges` método debe emitir una `INSERT` instrucción.
   > - `Unchanged`. No es necesario hacer nada con esta entidad mediante el método `SaveChanges`. Al leer una entidad de la base de datos, la entidad empieza con este estado.
   > - `Modified`. Se han modificado algunos o todos los valores de propiedad de la entidad. El `SaveChanges` método debe emitir una `UPDATE` instrucción.
   > - `Deleted`. La entidad se ha marcado para su eliminación. El `SaveChanges` método debe emitir una `DELETE` instrucción.
   > - `Detached`. El contexto de base de datos no está realizando el seguimiento de la entidad.
   >
   > En una aplicación de escritorio, los cambios de estado normalmente se establecen de forma automática. En un tipo de aplicación de escritorio leer una entidad y realizar cambios en algunos de sus valores de propiedad. Esto hace que su estado de entidad cambie automáticamente a `Modified`. A continuación, cuando se llama a `SaveChanges`, Entity Framework genera una instancia de SQL `UPDATE` instrucción que actualiza solo las propiedades reales que cambió.
   >
   > La naturaleza desconectada de web apps no permite que esta secuencia continua. El [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx) que lee una entidad se elimina después de representa una página. Cuando el `HttpPost` `Edit` se llama al método de acción, se realiza una solicitud nueva y tiene una nueva instancia de la [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx), por lo que debe establecer manualmente el estado de entidad en `Modified.` , a continuación, cuando se llama a `SaveChanges`, Entity Framework actualiza todas las columnas de la fila de la base de datos, porque el contexto no tiene ninguna manera de saber qué propiedades se han cambiado.
   >
   > Si desea que el código SQL `Update` instrucción para actualizar solo los campos que el usuario ha cambiado realmente, puede guardar los valores originales de alguna manera (por ejemplo, los campos ocultos) para que estén disponibles cuando el `HttpPost` `Edit` se llama al método. A continuación, puede crear un `Student` entidad utilizando los valores originales, llamada la `Attach` método con esa versión original de la entidad, actualice los valores de la entidad a los nuevos valores y, a continuación, llame a `SaveChanges.` para obtener más información, consulte [ Estados de entidad y SaveChanges](/ef/ef6/saving/change-tracking/entity-state) y [datos locales](/ef/ef6/querying/local-data).

   El código HTML y Razor en *Views\Student\Edit.cshtml* es similar a lo vimos en *Create.cshtml*, y se requiere ningún cambio.

2. Ejecute la página, inicie el programa, seleccione el **estudiantes** ficha y, a continuación, haga clic en un **editar** hyperlink.

3. Cambie algunos de los datos y haga clic en **Guardar**. Ver los datos modificados en la página de índice.

4. Cierre el explorador.

## <a name="update-the-delete-page"></a>Actualizar la página Delete

En *Controllers\StudentController.cs*, el código de plantilla para el <xref:System.Web.Mvc.HttpGetAttribute> `Delete` método usa la `Find` método para recuperar el texto seleccionado `Student` entidad, como se vio en el `Details` y `Edit` métodos. Pero para implementar un mensaje de error personalizado cuando se produce un error en la llamada a `SaveChanges`, agregará funcionalidad a este método y su vista correspondiente.

Como se vio para las operaciones de actualización y creación, las operaciones de eliminación requieren dos métodos de acción. El método que se llama en respuesta a una solicitud GET muestra una vista que proporciona al usuario una oportunidad para aprobar o cancelar la operación de eliminación. Si el usuario la aprueba, se crea una solicitud POST. Cuando esto ocurre, el `HttpPost` `Delete` se llama al método y, a continuación, ese método realmente realiza la operación de eliminación.

Agregará un `try-catch` bloquear a la <xref:System.Web.Mvc.HttpPostAttribute> `Delete` método para controlar los errores que pueden producirse cuando se actualiza la base de datos. Si se produce un error, el <xref:System.Web.Mvc.HttpPostAttribute> `Delete` llamadas al método el <xref:System.Web.Mvc.HttpGetAttribute> `Delete` método, pasando un parámetro que indica que se ha producido un error. El <xref:System.Web.Mvc.HttpGetAttribute> `Delete` método, a continuación, vuelve a mostrar la página de confirmación junto con el mensaje de error, dando al usuario la oportunidad de cancelar o vuelva a intentarlo.

1. Reemplace el <xref:System.Web.Mvc.HttpGetAttribute> `Delete` método de acción con el código siguiente, que administra el informe de errores:

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample12.cs?highlight=1,7-10)]

    Este código acepta un [parámetro opcional](https://msdn.microsoft.com/library/dd264739.aspx) que indica si se llamó al método después de un error al guardar los cambios. Este parámetro es `false` cuando el `HttpGet` `Delete` se llama al método sin un error anterior. Cuando se llama el `HttpPost` `Delete` método en respuesta a un error de actualización de la base de datos, el parámetro es `true` y un mensaje de error se pasa a la vista.

2. Reemplace el <xref:System.Web.Mvc.HttpPostAttribute> `Delete` método de acción (denominado `DeleteConfirmed`) con el código siguiente, que realiza la operación de eliminación y captura los errores de actualización de la base de datos.

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample13.cs)]

    Este código recupera la entidad seleccionada y, a continuación, llama a la [quitar](https://msdn.microsoft.com/library/system.data.entity.dbset.remove(v=vs.103).aspx) método para establecer el estado de la entidad `Deleted`. Cuando `SaveChanges` se denomina una instancia de SQL `DELETE` se genere un comando. También ha cambiado el nombre del método de acción de `DeleteConfirmed` a `Delete`. El nombre de código con scaffolding la `HttpPost` `Delete` método `DeleteConfirmed` para dar el `HttpPost` método una firma única. (El CLR requiere métodos sobrecargados para tener parámetros de método diferentes). Ahora que las firmas son únicas, puede seguir la convención de MVC y usar el mismo nombre para el `HttpPost` y `HttpGet` elimina métodos.

    Si mejorar el rendimiento en una aplicación de gran volumen es una prioridad, podría evitar una consulta SQL innecesaria para recuperar la fila reemplazando las líneas de código que llame a la `Find` y `Remove` métodos con el código siguiente:

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample14.cs)]

    Este código crea instancias de un `Student` entidad usando solo el valor de clave principal y, a continuación, Establece el estado de entidad en `Deleted`. Eso es todo lo que necesita Entity Framework para eliminar la entidad.

    Como se indicó, el `HttpGet` `Delete` método no elimina los datos. Realizar una operación de eliminación en respuesta a una operación GET de solicitud (o para este propósito efectuar cualquier operación de edición, creación o cualquier otra operación que cambia los datos) crea un riesgo de seguridad. Para obtener más información, consulte [ASP.NET MVC sugerencia #46; no utilice los vínculos eliminar ya que crean vulnerabilidades de seguridad](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx) en el blog de Stephen Walther.

3. En *Views\Student\Delete.cshtml*, agregar un mensaje de error entre el `h2` encabezado y el `h3` encabezado, tal como se muestra en el ejemplo siguiente:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample15.cshtml?highlight=2)]

4. Ejecute la página, inicie el programa, seleccione el **estudiantes** ficha y, a continuación, haga clic en un **eliminar** hyperlink.

5. Elija **eliminar** en la página que dice **¿está seguro de que desea eliminar esto?**.

    Muestra la página de índice sin el estudiante eliminado. (Verá un ejemplo de código en acción de control de errores el [tutorial de simultaneidad](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md).)

## <a name="close-database-connections"></a>Conexiones de cierre de la base de datos

Para cerrar las conexiones de base de datos y liberar los recursos que contienen tan pronto como sea posible, eliminar la instancia del contexto cuando haya terminado con él. ¿Por qué el código con scaffolding proporciona un [Dispose](https://msdn.microsoft.com/library/system.idisposable.dispose(v=vs.110).aspx) método al final de la `StudentController` clase *StudentController.cs*, como se muestra en el ejemplo siguiente:

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample16.cs)]

La base de `Controller` la clase ya implementa la `IDisposable` interfaz, por lo que este código simplemente agrega una invalidación para el `Dispose(bool)` método desecharlo de forma explícita la instancia del contexto.

## <a name="handle-transactions"></a>Controlar transacciones

De forma predeterminada, Entity Framework implementa las transacciones de manera implícita. En escenarios donde realizar cambios en varias filas o tablas y, a continuación, llame a `SaveChanges`, Entity Framework garantiza automáticamente que todos los cambios se realizan correctamente o producirá un error en todos. Si primero se realizan algunos cambios y después se produce un error, los cambios se revierten automáticamente. Para escenarios donde se necesita más control&mdash;por ejemplo, si desea incluir operaciones realizadas fuera de Entity Framework en una transacción&mdash;vea [trabajar con transacciones](/ef/ef6/saving/transactions).

## <a name="get-the-code"></a>Obtención del código

[Descargue el proyecto completado](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a>Recursos adicionales

Ahora tiene un conjunto completo de páginas que realizan operaciones CRUD sencillas para `Student` entidades. Aplicaciones auxiliares MVC se usan para generar elementos de interfaz de usuario para los campos de datos. Para obtener más información sobre las aplicaciones auxiliares MVC, consulte [representar un formulario utilizando aplicaciones auxiliares HTML](/previous-versions/aspnet/dd410596(v=vs.98)) (el artículo es para MVC 3, pero sigue siendo pertinente para MVC 5).

Pueden encontrar vínculos a otros recursos de EF 6 en [acceso a datos de ASP.NET - recursos recomendados](../../../../whitepapers/aspnet-data-access-content-map.md).

## <a name="next-steps"></a>Pasos siguientes

En este tutorial ha:

> [!div class="checklist"]
> * Crea una página de detalles
> * Actualiza la página Create
> * Actualiza el método HttpPost Edit
> * Actualiza la página Delete
> * Conexiones de base de datos cerradas
> * Transacciones procesadas

Avance al siguiente artículo para obtener información sobre cómo agregar ordenación, filtrado y paginación al proyecto.
> [!div class="nextstepaction"]
> [Ordenar, filtrar y paginar](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)
