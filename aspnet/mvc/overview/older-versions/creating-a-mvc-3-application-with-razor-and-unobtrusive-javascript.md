---
uid: mvc/overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript
title: Crear un MVC 3 aplicaciones con JavaScript Razor y discreto | Documentos de Microsoft
author: microsoft
description: "La aplicación web de ejemplo de lista de usuarios muestra lo fácil que es crear aplicaciones de ASP.NET MVC 3 con el motor de vista Razor. La s de aplicación de ejemplo..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/01/2010
ms.topic: article
ms.assetid: 658b149b-d770-46bf-8b4b-4e47cca242f3
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript
msc.type: authoredcontent
ms.openlocfilehash: 29b45c07b5498542abbf22c4c3001b1cee41edc9
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript"></a>Crear un MVC 3 aplicaciones con JavaScript Razor y discreto
====================
por [Microsoft](https://github.com/microsoft)

> La aplicación web de ejemplo de lista de usuarios muestra lo fácil que es crear aplicaciones de ASP.NET MVC 3 con el motor de vista Razor. La aplicación de ejemplo muestra cómo utilizar el nuevo motor de vista Razor con ASP.NET MVC versión 3 y Visual Studio 2010 para crear un sitio de Web de lista de usuarios ficticio que incluye una funcionalidad, como crear, mostrar, editar y eliminar usuarios.
> 
> Este tutorial describen los pasos que se realizaron para compilar la aplicación de ASP.NET MVC 3 de ejemplo de lista de usuarios. Un proyecto de Visual Studio con código fuente de C# y VB está disponible como acompañamiento de este tema: [descargar](https://code.msdn.microsoft.com/aspnetmvcsamples/Release/ProjectReleases.aspx?ReleaseId=5114). Si tiene alguna pregunta acerca de este tutorial, envíe a la [foro MVC](https://forums.asp.net/1146.aspx).


## <a name="overview"></a>Información general

La aplicación que creará es un sitio Web de lista de usuario sencilla. Los usuarios pueden escribir, ver y actualizar información de usuario.

![Sitio de ejemplo](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image1.png)

Puede descargar el proyecto completado de VB y C# [aquí](https://code.msdn.microsoft.com/Creating-a-MVC-3-28883c0f).

## <a name="creating-the-web-application"></a>Crear la aplicación Web

Para iniciar el tutorial, abra Visual Studio 2010 y crear un nuevo proyecto mediante el *aplicación Web de ASP.NET MVC 3* plantilla. Nombre de la aplicación &quot;Mvc3Razor&quot;.

[![Nuevo proyecto de MVC 3](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image3.png)](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image2.png)

En el **nuevo proyecto de ASP.NET MVC 3** cuadro de diálogo, seleccione **aplicación de Internet**, seleccione el motor de vista Razor y, a continuación, haga clic en **Aceptar**.

![Cuadro de diálogo nuevo proyecto de ASP.NET MVC 3](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image4.png)

En este tutorial no usará el proveedor de pertenencia ASP.NET, por lo que puede eliminar todos los archivos asociados con el inicio de sesión y pertenencia. En **el Explorador de soluciones**, eliminar los archivos y directorios siguientes:

- *Controllers\AccountController*
- *Models\AccountModels*
- *Views\Shared\\_LogOnPartial*
- *Views\Account* (y todos los archivos de este directorio)

![Soln Exp](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image5.png)

Editar la  *\_Layout.cshtml* archivo y reemplace el marcado dentro de la `<div>` elemento denominado `logindisplay` con el mensaje  *&quot;* inicio de sesión deshabilitado&quot;. En el ejemplo siguiente se muestra el marcado nuevo:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample1.cshtml)]

## <a name="adding-the-model"></a>Agregar el modelo

En **el Explorador de soluciones**, haga clic en el *modelos* carpeta, seleccione **agregar**y, a continuación, haga clic en **clase**.

![Nueva clase de usuario Mdl](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image6.png)

Asigne a la clase el nombre `UserModel`. Reemplace el contenido de la *UserModel* archivo con el código siguiente:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample2.cs)]

La `UserModel` clase representa a los usuarios. Cada miembro de la clase está anotada con la [requiere](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) de atributo de la [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) espacio de nombres. Los atributos de la [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) espacio de nombres proporcionan validación automática de lado cliente y servidor para aplicaciones web.

Abra la `HomeController` clase y agregue un `using` la directiva para que pueda acceder el `UserModel` y `Users` clases:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample3.cs)]

Justo después del `HomeController` declaración, agregue el siguiente comentario y la referencia a un `Users` clase:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample4.cs)]

La `Users` clase es un almacén de datos simplificado en memoria que va a utilizar en este tutorial. En una aplicación real, utilizaría una base de datos para almacenar información de usuario. Las primeras líneas de la `HomeController` archivo se muestran en el ejemplo siguiente:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample5.cs)]

Compile la aplicación para que el modelo de usuario estará disponible para el Asistente de técnica scaffolding en el paso siguiente.

## <a name="creating-the-default-view"></a>Crear la vista predeterminada

El siguiente paso es agregar un método de acción y una vista para mostrar los usuarios.

Eliminar las existentes *Views\Home\Index* archivo. Se creará una nueva *índice* archivo para mostrar los usuarios.

En el `HomeController` clase, reemplace el contenido de la `Index` método por el código siguiente:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample6.cs)]

Pulse el botón derecho dentro de la `Index` método y, a continuación, haga clic en **agregar vista**.

![Agregar vista](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image7.png)

Seleccione el **crear una vista fuertemente tipada** opción. Para **Ver clase de datos**, seleccione **Mvc3Razor.Models.UserModel**. (Si no ve **Mvc3Razor.Models.UserModel** en el **ver datos clase** cuadro, que necesita para compilar el proyecto.) Asegúrese de que el motor de vista se establece en **Razor**. Establecer **ver contenido** a **lista** y, a continuación, haga clic en **agregar**.

![Agregar vista de índice](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image8.png)

La nueva vista scaffolds automáticamente los datos de usuario que se pasan a la `Index` vista. Examine el recién generado *Views\Home\Index* archivo. El **crear nuevo**, **editar**, **detalles**, y **eliminar** los vínculos no funcionan, pero el resto de la página es funcional. Ejecute la página. Ver una lista de usuarios.

![Página de índice](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image9.png)

Abra la *Index.cshtml* archivo y reemplazar el `ActionLink` marcado para **editar**, **detalles**, y **eliminar** con el código siguiente :

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample7.cshtml)]

El nombre de usuario se utiliza como el Id. para encontrar el registro seleccionado en el **editar**, **detalles**, y **eliminar** vínculos.

## <a name="creating-the-details-view"></a>Crear la vista de detalles

El paso siguiente consiste en Agregar un `Details` método de acción y la vista para mostrar detalles del usuario.

![Detalles](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image10.png)

Agregue las siguientes `Details` método al controlador home:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample8.cs)]

Pulse el botón derecho dentro de la `Details` método y, a continuación, seleccione **agregar vista**. Compruebe que la **Ver clase de datos** cuadro contiene **Mvc3Razor.Models.UserModel***.* Establecer **ver contenido** a **detalles** y, a continuación, haga clic en **agregar**.

![Agregar la vista de detalles](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image11.png)

Ejecute la aplicación y seleccione un vínculo de detalles. La técnica scaffolding automática muestra cada propiedad en el modelo.

![Detalles](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image12.png)

## <a name="creating-the-edit-view"></a>Crear la vista de edición

Agregue las siguientes `Edit` método al controlador home.

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample9.cs)]

Agregar una vista como en los pasos anteriores, pero establecer **ver contenido** a **editar**.

![Agregar la vista de edición](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image13.png)

Ejecute la aplicación y modificar el nombre y apellido de uno de los usuarios. Si se infringe cualquiera `DataAnnotation` las restricciones que se han aplicado a la `UserModel` (clase), cuando se envía el formulario, verá errores de validación que se producen por el código del servidor. Por ejemplo, si cambia el nombre &quot;Ana&quot; a &quot;A&quot;, cuando se envía el formulario, aparece el error siguiente en el formulario:

`The field First Name must be a string with a minimum length of 3 and a maximum length of 8.`

En este tutorial, el nombre de usuario se tratan como clave principal. Por lo tanto, no se puede cambiar la propiedad de nombre de usuario. En el *Edit.cshtml* archivo, justo después del `Html.BeginForm` (instrucción), establezca el nombre de usuario como un campo oculto. Esto hace que la propiedad que se pasará en el modelo. El siguiente fragmento de código muestra la posición de la `Hidden` instrucción:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample10.cshtml)]

Reemplace el `TextBoxFor` y `ValidationMessageFor` marcado para el nombre de usuario con un `DisplayFor` llamar. El `DisplayFor` método muestra la propiedad como un elemento de solo lectura. En el ejemplo siguiente se muestra el marcado completado. La versión original `TextBoxFor` y `ValidationMessageFor` llamadas se acota entre los caracteres de comentario begin y end comentario de Razor (`@* *@`)

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample11.cshtml)]

## <a name="enabling-client-side-validation"></a>Habilitar la validación del lado cliente

Para habilitar la validación del lado cliente en ASP.NET MVC 3, debe establecer dos marcas y debe incluir tres archivos de JavaScript.

Abra la aplicación *Web.config* archivo. Comprobar `that ClientValidationEnabled` y `UnobtrusiveJavaScriptEnabled` se establece en true en la configuración de la aplicación. El siguiente fragmento de la raíz *Web.config* archivo muestra la configuración correcta:

[!code-xml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample12.xml)]

Establecer `UnobtrusiveJavaScriptEnabled` en true habilita discreto Ajax y validación del cliente discreta. Cuando se utiliza validación discreto, las reglas de validación se convierten en atributos de HTML5. Nombres de atributo de HTML5 pueden constar de solo letras minúsculas, números y guiones.

Establecer `ClientValidationEnabled` a true habilita la validación del lado cliente. Mediante el establecimiento de estas claves en la aplicación *Web.config* archivo, habilitar la validación del cliente y JavaScript discreto para toda la aplicación. También puede habilitar o deshabilitar a esta configuración en las vistas individuales o en métodos de controlador con el código siguiente:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample13.cs)]

También deberá incluir varios archivos de JavaScript en la vista representada. Es una manera fácil de incluir el código JavaScript en todas las vistas agregarlos a la *Views\Shared\\_Layout.cshtml* archivo. Reemplace el `<head>` elemento de la  *\_Layout.cshtml* archivo con el código siguiente:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample14.cshtml)]

Los dos primeros scripts de jQuery se hospedan mediante la Microsoft Ajax entrega red contenido (CDN). Aprovechando las ventajas de la CDN de Microsoft Ajax, notablemente puede mejorar el rendimiento de aciertos de la primera de las aplicaciones.

Ejecute la aplicación y haga clic en un vínculo de edición. Ver código fuente de la página en el explorador. El código fuente del explorador muestra todos los atributos del formulario `data-val` (para la validación de datos). Cuando se habilita la validación del cliente y JavaScript discreto, contienen campos de entrada con una regla de validación del cliente la `data-val="true"` atributo para desencadenar la validación de cliente discretos. Por ejemplo, el `City` campo en el modelo se decora con el [necesario](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) atributo, lo que resulta en el código HTML que se muestra en el ejemplo siguiente:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample15.cshtml)]

Para cada regla de validación del cliente, se agrega un atributo que tiene el formato `data-val-rulename="message"`. Mediante el `City` ejemplo campo mostrado anteriormente, la regla de validación de cliente necesarias genera el `data-val-required` atributo y el mensaje &quot;ciudad el campo es obligatorio&quot;. Ejecute la aplicación, modificar uno de los usuarios y desactive el `City` campo. Cuando salga del campo, verá un mensaje de error de validación del lado cliente.

![Ciudad necesario](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image14.png)

De forma similar, para cada parámetro de la regla de validación del cliente, se agrega un atributo que tiene el formato `data-val-rulename-paramname=paramvalue`. Por ejemplo, el `FirstName` propiedad se anota con el [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) de atributo y especifica una longitud mínima de 3 y una longitud máxima de 8. La regla de validación de datos denominada `length` tiene el nombre del parámetro `max` y el valor del parámetro 8. La siguiente muestra el código HTML que se genera para el `FirstName` campo cuando se modifica uno de los usuarios:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample16.cshtml)]

Para obtener más información acerca de la validación de cliente discretos, vea la entrada [discreto validación del cliente en ASP.NET MVC 3](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html) en el blog de Brad Wilson.

> [!NOTE]
> En la versión Beta de ASP.NET MVC 3, a veces es necesario enviar el formulario con el fin de iniciar la validación del lado cliente. Esto se puede cambiar para la versión final.


## <a name="creating-the-create-view"></a>Crear la vista de creación

El paso siguiente consiste en Agregar un `Create` método de acción y la vista para permitir que el usuario crear un nuevo usuario. Agregue las siguientes `Create` método al controlador home:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample17.cs)]

Agregar una vista como en los pasos anteriores, pero establecer **ver contenido** a **crear**.

![Crear vista](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image15.png)

Ejecute la aplicación, seleccione la **crear** vincular y agregar un nuevo usuario. El `Create` método automáticamente aprovecha las ventajas de validación de cliente y servidor. Intenta escribir un nombre de usuario que contiene espacios en blanco, como &quot;Ben X&quot;. Cuando salga del campo de nombre de usuario, un error de validación del lado cliente (`White space is not allowed`) se muestra.

## <a name="add-the-delete-method"></a>Agregue el método de eliminación

Para completar el tutorial, agregue las siguientes `Delete` método al controlador home:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample18.cs)]

Agregar un `Delete` vista como en los pasos anteriores, establecer **ver contenido** a **eliminar**.

![Eliminar vista](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image16.png)

Ahora tiene una aplicación de ASP.NET MVC 3 simple pero totalmente funcional con la validación.
