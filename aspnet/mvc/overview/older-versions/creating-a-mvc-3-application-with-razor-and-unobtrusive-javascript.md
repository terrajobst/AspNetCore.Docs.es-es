---
uid: mvc/overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript
title: Crear un MVC 3 aplicaciones con JavaScript Razor y discreto | Microsoft Docs
author: microsoft
description: La aplicación web de lista de usuarios muestra lo sencillo que es crear aplicaciones de ASP.NET MVC 3 con el motor de vistas Razor. La s de aplicación de ejemplo...
ms.author: riande
ms.date: 11/01/2010
ms.assetid: 658b149b-d770-46bf-8b4b-4e47cca242f3
msc.legacyurl: /mvc/overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript
msc.type: authoredcontent
ms.openlocfilehash: f60ca3e76fda8a18da1ad83e955e5e4759f5e3af
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829857"
---
<a name="creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript"></a><span data-ttu-id="53cf9-104">Crear un MVC 3 aplicaciones con JavaScript Razor y discreto</span><span class="sxs-lookup"><span data-stu-id="53cf9-104">Creating a MVC 3 Application with Razor and Unobtrusive JavaScript</span></span>
====================
<span data-ttu-id="53cf9-105">por [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="53cf9-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="53cf9-106">La aplicación web de lista de usuarios muestra lo sencillo que es crear aplicaciones de ASP.NET MVC 3 con el motor de vistas Razor.</span><span class="sxs-lookup"><span data-stu-id="53cf9-106">The User List sample web application demonstrates how simple it is to create ASP.NET MVC 3 applications using the Razor view engine.</span></span> <span data-ttu-id="53cf9-107">La aplicación de ejemplo muestra cómo usar el nuevo motor de vista de Razor con ASP.NET MVC versión 3 y Visual Studio 2010 para crear un sitio Web lista de usuarios ficticio que incluye funciones como la creación, visualización, edición y eliminación de usuarios.</span><span class="sxs-lookup"><span data-stu-id="53cf9-107">The sample application shows how to use the new Razor view engine with ASP.NET MVC version 3 and Visual Studio 2010 to create a fictional User List website that includes functionality such as creating, displaying, editing, and deleting users.</span></span>
> 
> <span data-ttu-id="53cf9-108">En este tutorial se describe los pasos que se llevaron a cabo con el fin de compilar la aplicación de ASP.NET MVC 3 de ejemplo de lista de usuarios.</span><span class="sxs-lookup"><span data-stu-id="53cf9-108">This tutorial describes the steps that were taken in order to build the User List sample ASP.NET MVC 3 application.</span></span> <span data-ttu-id="53cf9-109">Un proyecto de Visual Studio con código fuente de C# y VB está disponible como acompañamiento de este tema: [descargar](https://code.msdn.microsoft.com/aspnetmvcsamples/Release/ProjectReleases.aspx?ReleaseId=5114).</span><span class="sxs-lookup"><span data-stu-id="53cf9-109">A Visual Studio project with C# and VB source code is available to accompany this topic: [Download](https://code.msdn.microsoft.com/aspnetmvcsamples/Release/ProjectReleases.aspx?ReleaseId=5114).</span></span> <span data-ttu-id="53cf9-110">Si tiene preguntas acerca de este tutorial, publíquelas en el [foro MVC](https://forums.asp.net/1146.aspx).</span><span class="sxs-lookup"><span data-stu-id="53cf9-110">If you have questions about this tutorial, please post them to the [MVC forum](https://forums.asp.net/1146.aspx).</span></span>


## <a name="overview"></a><span data-ttu-id="53cf9-111">Información general</span><span class="sxs-lookup"><span data-stu-id="53cf9-111">Overview</span></span>

<span data-ttu-id="53cf9-112">La aplicación que se va a compilar es un sitio Web de lista de usuario sencilla.</span><span class="sxs-lookup"><span data-stu-id="53cf9-112">The application you'll be building is a simple user list website.</span></span> <span data-ttu-id="53cf9-113">Los usuarios pueden introducir, ver y actualizar la información de usuario.</span><span class="sxs-lookup"><span data-stu-id="53cf9-113">Users can enter, view, and update user information.</span></span>

![Sitio de ejemplo](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image1.png)

<span data-ttu-id="53cf9-115">Puede descargar el proyecto completado de VB y C# [aquí](https://code.msdn.microsoft.com/Creating-a-MVC-3-28883c0f).</span><span class="sxs-lookup"><span data-stu-id="53cf9-115">You can download the VB and C# completed project [here](https://code.msdn.microsoft.com/Creating-a-MVC-3-28883c0f).</span></span>

## <a name="creating-the-web-application"></a><span data-ttu-id="53cf9-116">Creación de la aplicación Web</span><span class="sxs-lookup"><span data-stu-id="53cf9-116">Creating the Web Application</span></span>

<span data-ttu-id="53cf9-117">Para iniciar el tutorial, abra Visual Studio 2010 y cree un nuevo proyecto con el *aplicación Web de ASP.NET MVC 3* plantilla.</span><span class="sxs-lookup"><span data-stu-id="53cf9-117">To start the tutorial, open Visual Studio 2010 and create a new project using the *ASP.NET MVC 3 Web Application* template.</span></span> <span data-ttu-id="53cf9-118">Nombre de la aplicación &quot;Mvc3Razor&quot;.</span><span class="sxs-lookup"><span data-stu-id="53cf9-118">Name the application &quot;Mvc3Razor&quot;.</span></span>

<span data-ttu-id="53cf9-119">[![Nuevo proyecto de MVC 3](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image3.png)](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image2.png)</span><span class="sxs-lookup"><span data-stu-id="53cf9-119">[![New MVC 3 project](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image3.png)](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image2.png)</span></span>

<span data-ttu-id="53cf9-120">En el **nuevo proyecto de ASP.NET MVC 3** cuadro de diálogo, seleccione **aplicación de Internet**, seleccione el motor de vistas Razor y, a continuación, haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="53cf9-120">In the **New ASP.NET MVC 3 Project** dialog, select **Internet Application**, select the Razor view engine, and then click **OK**.</span></span>

![Cuadro de diálogo nuevo proyecto ASP.NET MVC 3](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image4.png)

<span data-ttu-id="53cf9-122">En este tutorial no usará el proveedor de pertenencia ASP.NET, por lo que puede eliminar todos los archivos asociados con el inicio de sesión y la pertenencia.</span><span class="sxs-lookup"><span data-stu-id="53cf9-122">In this tutorial you will not be using the ASP.NET membership provider, so you can delete all the files associated with logon and membership.</span></span> <span data-ttu-id="53cf9-123">En **el Explorador de soluciones**, quite los archivos y directorios siguientes:</span><span class="sxs-lookup"><span data-stu-id="53cf9-123">In **Solution Explorer**, remove the following files and directories:</span></span>

- <span data-ttu-id="53cf9-124">*Controllers\AccountController*</span><span class="sxs-lookup"><span data-stu-id="53cf9-124">*Controllers\AccountController*</span></span>
- <span data-ttu-id="53cf9-125">*Models\AccountModels*</span><span class="sxs-lookup"><span data-stu-id="53cf9-125">*Models\AccountModels*</span></span>
- <span data-ttu-id="53cf9-126">*Views\Shared\\_LogOnPartial*</span><span class="sxs-lookup"><span data-stu-id="53cf9-126">*Views\Shared\\_LogOnPartial*</span></span>
- <span data-ttu-id="53cf9-127">*Views\Account* (y todos los archivos de este directorio)</span><span class="sxs-lookup"><span data-stu-id="53cf9-127">*Views\Account* (and all the files in this directory)</span></span>

![Exp soln](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image5.png)

<span data-ttu-id="53cf9-129">Editar el  <em>\_Layout.cshtml</em> de archivo y reemplace el marcado dentro de la `<div>` elemento denominado `logindisplay` con el mensaje <em>&quot;</em>inicio de sesión deshabilitado&quot;.</span><span class="sxs-lookup"><span data-stu-id="53cf9-129">Edit the <em>\_Layout.cshtml</em> file and replace the markup inside the `<div>` element named `logindisplay` with the message <em>&quot;</em>Login Disabled&quot;.</span></span> <span data-ttu-id="53cf9-130">El ejemplo siguiente muestra el nuevo marcado:</span><span class="sxs-lookup"><span data-stu-id="53cf9-130">The following example shows the new markup:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample1.cshtml)]

## <a name="adding-the-model"></a><span data-ttu-id="53cf9-131">Agregar el modelo</span><span class="sxs-lookup"><span data-stu-id="53cf9-131">Adding the Model</span></span>

<span data-ttu-id="53cf9-132">En **el Explorador de soluciones**, haga clic en el *modelos* carpeta, seleccione **agregar**y, a continuación, haga clic en **clase**.</span><span class="sxs-lookup"><span data-stu-id="53cf9-132">In **Solution Explorer**, right-click the *Models* folder, select **Add**, and then click **Class**.</span></span>

![Nueva clase de usuario Mdl](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image6.png)

<span data-ttu-id="53cf9-134">Asigne a la clase el nombre `UserModel`.</span><span class="sxs-lookup"><span data-stu-id="53cf9-134">Name the class `UserModel`.</span></span> <span data-ttu-id="53cf9-135">Reemplace el contenido de la *UserModel* archivo con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="53cf9-135">Replace the contents of the *UserModel* file with the following code:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample2.cs)]

<span data-ttu-id="53cf9-136">La `UserModel` clase representa a los usuarios.</span><span class="sxs-lookup"><span data-stu-id="53cf9-136">The `UserModel` class represents users.</span></span> <span data-ttu-id="53cf9-137">Cada miembro de la clase está anotada con la [requiere](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) atributo desde el [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="53cf9-137">Each member of the class is annotated with the [Required](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) attribute from the [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) namespace.</span></span> <span data-ttu-id="53cf9-138">Los atributos en el [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) espacio de nombres proporcionan validación automática de lado cliente y servidor para las aplicaciones web.</span><span class="sxs-lookup"><span data-stu-id="53cf9-138">The attributes in the [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) namespace provide automatic client- and server-side validation for web applications.</span></span>

<span data-ttu-id="53cf9-139">Abra el `HomeController` clase y agregue un `using` la directiva para que puede acceder a la `UserModel` y `Users` clases:</span><span class="sxs-lookup"><span data-stu-id="53cf9-139">Open the `HomeController` class and add a `using` directive so that you can access the `UserModel` and `Users` classes:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample3.cs)]

<span data-ttu-id="53cf9-140">Justo después del `HomeController` declaración, agregue el siguiente comentario y la referencia a un `Users` clase:</span><span class="sxs-lookup"><span data-stu-id="53cf9-140">Just after the `HomeController` declaration, add the following comment and the reference to a `Users` class:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample4.cs)]

<span data-ttu-id="53cf9-141">La `Users` clase es un almacén de datos simplificada, en memoria que va a usar en este tutorial.</span><span class="sxs-lookup"><span data-stu-id="53cf9-141">The `Users` class is a simplified, in-memory data store that you'll use in this tutorial.</span></span> <span data-ttu-id="53cf9-142">En una aplicación real podría usar una base de datos para almacenar información de usuario.</span><span class="sxs-lookup"><span data-stu-id="53cf9-142">In a real application you would use a database to store user information.</span></span> <span data-ttu-id="53cf9-143">Las primeras líneas de la `HomeController` archivo se muestran en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="53cf9-143">The first few lines of the `HomeController` file are shown in the following example:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample5.cs)]

<span data-ttu-id="53cf9-144">Compile la aplicación para que el modelo de usuario estará disponible para el Asistente de scaffolding en el paso siguiente.</span><span class="sxs-lookup"><span data-stu-id="53cf9-144">Build the application so that the user model will be available to the scaffolding wizard in the next step.</span></span>

## <a name="creating-the-default-view"></a><span data-ttu-id="53cf9-145">Creación de la vista predeterminada</span><span class="sxs-lookup"><span data-stu-id="53cf9-145">Creating the Default View</span></span>

<span data-ttu-id="53cf9-146">El siguiente paso es agregar un método de acción y una vista para mostrar los usuarios.</span><span class="sxs-lookup"><span data-stu-id="53cf9-146">The next step is to add an action method and view to display the users.</span></span>

<span data-ttu-id="53cf9-147">Eliminar el existente *Views\Home\Index* archivo.</span><span class="sxs-lookup"><span data-stu-id="53cf9-147">Delete the existing *Views\Home\Index* file.</span></span> <span data-ttu-id="53cf9-148">Se creará una nueva *índice* archivo para mostrar los usuarios.</span><span class="sxs-lookup"><span data-stu-id="53cf9-148">You will create a new *Index* file to display the users.</span></span>

<span data-ttu-id="53cf9-149">En el `HomeController` class, reemplace el contenido de la `Index` método con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="53cf9-149">In the `HomeController` class, replace the contents of the `Index` method with the following code:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample6.cs)]

<span data-ttu-id="53cf9-150">Haga clic en el `Index` método y, a continuación, haga clic en **agregar vista**.</span><span class="sxs-lookup"><span data-stu-id="53cf9-150">Right-click inside the `Index` method and then click **Add View**.</span></span>

![Agregar vista](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image7.png)

<span data-ttu-id="53cf9-152">Seleccione el **crear una vista fuertemente tipada** opción.</span><span class="sxs-lookup"><span data-stu-id="53cf9-152">Select the **Create a strongly-typed view** option.</span></span> <span data-ttu-id="53cf9-153">Para **Ver clase de datos**, seleccione **Mvc3Razor.Models.UserModel**.</span><span class="sxs-lookup"><span data-stu-id="53cf9-153">For **View data class**, select **Mvc3Razor.Models.UserModel**.</span></span> <span data-ttu-id="53cf9-154">(Si no ve **Mvc3Razor.Models.UserModel** en el **Ver clase de datos** cuadro, deberá compilar el proyecto.) Asegúrese de que el motor de vistas está establecido en **Razor**.</span><span class="sxs-lookup"><span data-stu-id="53cf9-154">(If you don't see **Mvc3Razor.Models.UserModel** in the **View data class** box, you need to build the project.) Make sure that the view engine is set to **Razor**.</span></span> <span data-ttu-id="53cf9-155">Establecer **ver contenido** a **lista** y, a continuación, haga clic en **agregar**.</span><span class="sxs-lookup"><span data-stu-id="53cf9-155">Set **View content** to **List** and then click **Add**.</span></span>

![Agregar vista de índice](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image8.png)

<span data-ttu-id="53cf9-157">La nueva vista automáticamente aplica la técnica scaffolding los datos de usuario que se pasan a la `Index` vista.</span><span class="sxs-lookup"><span data-stu-id="53cf9-157">The new view automatically scaffolds the user data that's passed to the `Index` view.</span></span> <span data-ttu-id="53cf9-158">Examinar recién generado *Views\Home\Index* archivo.</span><span class="sxs-lookup"><span data-stu-id="53cf9-158">Examine the newly generated *Views\Home\Index* file.</span></span> <span data-ttu-id="53cf9-159">El **crear nuevo**, **editar**, **detalles**, y **eliminar** los vínculos no funcionan, pero el resto de la página es funcional.</span><span class="sxs-lookup"><span data-stu-id="53cf9-159">The **Create New**, **Edit**, **Details**, and **Delete** links don't work, but the rest of the page is functional.</span></span> <span data-ttu-id="53cf9-160">Ejecute la página.</span><span class="sxs-lookup"><span data-stu-id="53cf9-160">Run the page.</span></span> <span data-ttu-id="53cf9-161">Vea una lista de usuarios.</span><span class="sxs-lookup"><span data-stu-id="53cf9-161">You see a list of users.</span></span>

![Página de índice](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image9.png)

<span data-ttu-id="53cf9-163">Abra el *Index.cshtml* de archivo y reemplace el `ActionLink` marcado para **editar**, **detalles**, y **eliminar** con el código siguiente :</span><span class="sxs-lookup"><span data-stu-id="53cf9-163">Open the *Index.cshtml* file and replace the `ActionLink` markup for **Edit**, **Details**, and **Delete** with the following code:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample7.cshtml)]

<span data-ttu-id="53cf9-164">El nombre de usuario se utiliza como el identificador para buscar el registro seleccionado en el **editar**, **detalles**, y **eliminar** vínculos.</span><span class="sxs-lookup"><span data-stu-id="53cf9-164">The user name is used as the ID to find the selected record in the **Edit**, **Details**, and **Delete** links.</span></span>

## <a name="creating-the-details-view"></a><span data-ttu-id="53cf9-165">Creación de la vista de detalles</span><span class="sxs-lookup"><span data-stu-id="53cf9-165">Creating the Details View</span></span>

<span data-ttu-id="53cf9-166">El siguiente paso es agregar un `Details` método de acción y vista para mostrar los detalles del usuario.</span><span class="sxs-lookup"><span data-stu-id="53cf9-166">The next step is to add a `Details` action method and view in order to display user details.</span></span>

![Detalles](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image10.png)

<span data-ttu-id="53cf9-168">Agregue las siguientes `Details` método al controlador home:</span><span class="sxs-lookup"><span data-stu-id="53cf9-168">Add the following `Details` method to the home controller:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample8.cs)]

<span data-ttu-id="53cf9-169">Haga clic en el `Details` método y, a continuación, seleccione <strong>agregar vista</strong>.</span><span class="sxs-lookup"><span data-stu-id="53cf9-169">Right-click inside the `Details` method and then select <strong>Add View</strong>.</span></span> <span data-ttu-id="53cf9-170">Compruebe que la <strong>Ver clase de datos</strong> cuadro contiene <strong>Mvc3Razor.Models.UserModel</strong><em>.</em></span><span class="sxs-lookup"><span data-stu-id="53cf9-170">Verify that the <strong>View data class</strong> box contains <strong>Mvc3Razor.Models.UserModel</strong><em>.</em></span></span> <span data-ttu-id="53cf9-171">Establecer <strong>ver contenido</strong> a <strong>detalles</strong> y, a continuación, haga clic en <strong>agregar</strong>.</span><span class="sxs-lookup"><span data-stu-id="53cf9-171">Set <strong>View content</strong> to <strong>Details</strong> and then click <strong>Add</strong>.</span></span>

![Agregar vista de detalles](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image11.png)

<span data-ttu-id="53cf9-173">Ejecute la aplicación y seleccione un vínculo details.</span><span class="sxs-lookup"><span data-stu-id="53cf9-173">Run the application and select a details link.</span></span> <span data-ttu-id="53cf9-174">El scaffolding automático muestra cada propiedad en el modelo.</span><span class="sxs-lookup"><span data-stu-id="53cf9-174">The automatic scaffolding shows each property in the model.</span></span>

![Detalles](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image12.png)

## <a name="creating-the-edit-view"></a><span data-ttu-id="53cf9-176">Creación de la vista de edición</span><span class="sxs-lookup"><span data-stu-id="53cf9-176">Creating the Edit View</span></span>

<span data-ttu-id="53cf9-177">Agregue las siguientes `Edit` método al controlador home.</span><span class="sxs-lookup"><span data-stu-id="53cf9-177">Add the following `Edit` method to the home controller.</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample9.cs)]

<span data-ttu-id="53cf9-178">Agrega una vista como en los pasos anteriores, pero establece **ver contenido** a **editar**.</span><span class="sxs-lookup"><span data-stu-id="53cf9-178">Add a view as in the previous steps, but set **View content** to **Edit**.</span></span>

![Agregar vista de edición](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image13.png)

<span data-ttu-id="53cf9-180">Ejecute la aplicación y edite el nombre y apellido de uno de los usuarios.</span><span class="sxs-lookup"><span data-stu-id="53cf9-180">Run the application and edit the first and last name of one of the users.</span></span> <span data-ttu-id="53cf9-181">Si se infringe alguna `DataAnnotation` restricciones que se han aplicado a la `UserModel` (clase), cuando se envía el formulario, verá errores de validación producidos por el código del servidor.</span><span class="sxs-lookup"><span data-stu-id="53cf9-181">If you violate any `DataAnnotation` constraints that have been applied to the `UserModel` class, when you submit the form, you will see validation errors that are produced by server code.</span></span> <span data-ttu-id="53cf9-182">Por ejemplo, si cambia el nombre &quot;Ann&quot; a &quot;A&quot;, cuando se envía el formulario, se muestra el siguiente error en el formulario:</span><span class="sxs-lookup"><span data-stu-id="53cf9-182">For example, if you change the first name &quot;Ann&quot; to &quot;A&quot;, when you submit the form, the following error is displayed on the form:</span></span>

`The field First Name must be a string with a minimum length of 3 and a maximum length of 8.`

<span data-ttu-id="53cf9-183">En este tutorial, está tratando el nombre de usuario como clave principal.</span><span class="sxs-lookup"><span data-stu-id="53cf9-183">In this tutorial, you're treating the user name as the primary key.</span></span> <span data-ttu-id="53cf9-184">Por lo tanto, no se puede cambiar la propiedad de nombre de usuario.</span><span class="sxs-lookup"><span data-stu-id="53cf9-184">Therefore, the user name property cannot be changed.</span></span> <span data-ttu-id="53cf9-185">En el *Edit.cshtml* archivo, justo después del `Html.BeginForm` instrucción, establezca el nombre de usuario para que sea un campo oculto.</span><span class="sxs-lookup"><span data-stu-id="53cf9-185">In the *Edit.cshtml* file, just after the `Html.BeginForm` statement, set the user name to be a hidden field.</span></span> <span data-ttu-id="53cf9-186">Esto hace que la propiedad que se pasará en el modelo.</span><span class="sxs-lookup"><span data-stu-id="53cf9-186">This causes the property to be passed in the model.</span></span> <span data-ttu-id="53cf9-187">El fragmento de código siguiente muestra la posición de la `Hidden` instrucción:</span><span class="sxs-lookup"><span data-stu-id="53cf9-187">The following code fragment shows the placement of the `Hidden` statement:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample10.cshtml)]

<span data-ttu-id="53cf9-188">Reemplace el `TextBoxFor` y `ValidationMessageFor` marcado para el nombre de usuario con un `DisplayFor` llamar.</span><span class="sxs-lookup"><span data-stu-id="53cf9-188">Replace the `TextBoxFor` and `ValidationMessageFor` markup for the user name with a `DisplayFor` call.</span></span> <span data-ttu-id="53cf9-189">El `DisplayFor` método muestra la propiedad como un elemento de solo lectura.</span><span class="sxs-lookup"><span data-stu-id="53cf9-189">The `DisplayFor` method displays the property as a read-only element.</span></span> <span data-ttu-id="53cf9-190">El ejemplo siguiente muestra el marcado completado.</span><span class="sxs-lookup"><span data-stu-id="53cf9-190">The following example shows the completed markup.</span></span> <span data-ttu-id="53cf9-191">La versión original `TextBoxFor` y `ValidationMessageFor` llamadas están comentadas con los caracteres de comentario begin y end comentario de Razor (`@* *@`)</span><span class="sxs-lookup"><span data-stu-id="53cf9-191">The original `TextBoxFor` and `ValidationMessageFor` calls are commented out with the Razor begin-comment and end-comment characters (`@* *@`)</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample11.cshtml)]

## <a name="enabling-client-side-validation"></a><span data-ttu-id="53cf9-192">Habilitar la validación del lado cliente</span><span class="sxs-lookup"><span data-stu-id="53cf9-192">Enabling Client-Side Validation</span></span>

<span data-ttu-id="53cf9-193">Para habilitar la validación del lado cliente en ASP.NET MVC 3, debe establecer dos marcas y debe incluir tres archivos de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="53cf9-193">To enable client-side validation in ASP.NET MVC 3, you must set two flags and you must include three JavaScript files.</span></span>

<span data-ttu-id="53cf9-194">Abra la aplicación *Web.config* archivo.</span><span class="sxs-lookup"><span data-stu-id="53cf9-194">Open the application's *Web.config* file.</span></span> <span data-ttu-id="53cf9-195">Comprobar `that ClientValidationEnabled` y `UnobtrusiveJavaScriptEnabled` se establece en true en la configuración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="53cf9-195">Verify `that ClientValidationEnabled` and `UnobtrusiveJavaScriptEnabled` are set to true in the application settings.</span></span> <span data-ttu-id="53cf9-196">El fragmento siguiente de la raíz *Web.config* archivo muestra la configuración correcta:</span><span class="sxs-lookup"><span data-stu-id="53cf9-196">The following fragment from the root *Web.config* file shows the correct settings:</span></span>

[!code-xml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample12.xml)]

<span data-ttu-id="53cf9-197">Establecer `UnobtrusiveJavaScriptEnabled` en true habilita discreto Ajax y validación del cliente discreta.</span><span class="sxs-lookup"><span data-stu-id="53cf9-197">Setting `UnobtrusiveJavaScriptEnabled` to true enables unobtrusive Ajax and unobtrusive client validation.</span></span> <span data-ttu-id="53cf9-198">Cuando se usa validación discreta, las reglas de validación se convierten en atributos de HTML5.</span><span class="sxs-lookup"><span data-stu-id="53cf9-198">When you use unobtrusive validation, the validation rules are turned into HTML5 attributes.</span></span> <span data-ttu-id="53cf9-199">Los nombres de atributo de HTML5 pueden constar de solo letras minúsculas, números y guiones.</span><span class="sxs-lookup"><span data-stu-id="53cf9-199">HTML5 attribute names can consist of only lowercase letters, numbers, and dashes.</span></span>

<span data-ttu-id="53cf9-200">Establecer `ClientValidationEnabled` a true habilita la validación del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="53cf9-200">Setting `ClientValidationEnabled` to true enables client-side validation.</span></span> <span data-ttu-id="53cf9-201">Mediante el establecimiento de estas claves en la aplicación *Web.config* archivo, habilitar la validación del cliente y el JavaScript discreto para toda la aplicación.</span><span class="sxs-lookup"><span data-stu-id="53cf9-201">By setting these keys in the application *Web.config* file, you enable client validation and unobtrusive JavaScript for the entire application.</span></span> <span data-ttu-id="53cf9-202">También puede habilitar o deshabilitar a esta configuración en las vistas individuales o en los métodos de controlador mediante el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="53cf9-202">You can also enable or disable these settings in individual views or in controller methods using the following code:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample13.cs)]

<span data-ttu-id="53cf9-203">También deberá incluir varios archivos de JavaScript en la vista representada.</span><span class="sxs-lookup"><span data-stu-id="53cf9-203">You also need to include several JavaScript files in the rendered view.</span></span> <span data-ttu-id="53cf9-204">Es una manera fácil de incluir el código JavaScript en todas las vistas agregarlos a la *Views\Shared\\_Layout.cshtml* archivo.</span><span class="sxs-lookup"><span data-stu-id="53cf9-204">An easy way to include the JavaScript in all views is to add them to the *Views\Shared\\_Layout.cshtml* file.</span></span> <span data-ttu-id="53cf9-205">Reemplace el `<head>` elemento de la  *\_Layout.cshtml* archivo con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="53cf9-205">Replace the `<head>` element of the *\_Layout.cshtml* file with the following code:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample14.cshtml)]

<span data-ttu-id="53cf9-206">Los dos primeros scripts de jQuery se hospedan mediante la Microsoft Ajax Content Delivery Network (CDN).</span><span class="sxs-lookup"><span data-stu-id="53cf9-206">The first two jQuery scripts are hosted by the Microsoft Ajax Content Delivery Network (CDN).</span></span> <span data-ttu-id="53cf9-207">Aprovechando las ventajas de la red CDN de Microsoft Ajax, puede mejorar significativamente el rendimiento de la primera visita de sus aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="53cf9-207">By taking advantage of the Microsoft Ajax CDN, you can significantly improve the first-hit performance of your applications.</span></span>

<span data-ttu-id="53cf9-208">Ejecute la aplicación y haga clic en un vínculo de edición.</span><span class="sxs-lookup"><span data-stu-id="53cf9-208">Run the application and click an edit link.</span></span> <span data-ttu-id="53cf9-209">Ver código fuente de la página en el explorador.</span><span class="sxs-lookup"><span data-stu-id="53cf9-209">View the page's source in the browser.</span></span> <span data-ttu-id="53cf9-210">El código fuente del explorador muestra muchos de los atributos del formulario `data-val` (para la validación de datos).</span><span class="sxs-lookup"><span data-stu-id="53cf9-210">The browser source shows many attributes of the form `data-val` (for data validation).</span></span> <span data-ttu-id="53cf9-211">Cuando se habilita JavaScript discreto y validación de cliente, los campos de entrada con una regla de validación de cliente contienen el `data-val="true"` atributo para desencadenar la validación de cliente discretos.</span><span class="sxs-lookup"><span data-stu-id="53cf9-211">When client validation and unobtrusive JavaScript is enabled, input fields with a client-validation rule contain the `data-val="true"` attribute to trigger unobtrusive client validation.</span></span> <span data-ttu-id="53cf9-212">Por ejemplo, el `City` campo en el modelo se había decorado con el [necesario](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) atributo, lo que resulta en el código HTML que se muestra en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="53cf9-212">For example, the `City` field in the model was decorated with the [Required](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) attribute, which results in the HTML shown in the following example:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample15.cshtml)]

<span data-ttu-id="53cf9-213">Para cada regla de validación del cliente, se agrega un atributo que tiene el formato `data-val-rulename="message"`.</span><span class="sxs-lookup"><span data-stu-id="53cf9-213">For each client-validation rule, an attribute is added that has the form `data-val-rulename="message"`.</span></span> <span data-ttu-id="53cf9-214">Mediante el `City` ejemplo campo mostrado anteriormente, la regla de validación de cliente necesarias genera el `data-val-required` atributo y el mensaje &quot;ciudad el campo es obligatorio&quot;.</span><span class="sxs-lookup"><span data-stu-id="53cf9-214">Using the `City` field example shown earlier, the required client-validation rule generates the `data-val-required` attribute and the message &quot;The City field is required&quot;.</span></span> <span data-ttu-id="53cf9-215">Ejecute la aplicación, editar uno de los usuarios y desactive el `City` campo.</span><span class="sxs-lookup"><span data-stu-id="53cf9-215">Run the application, edit one of the users, and clear the `City` field.</span></span> <span data-ttu-id="53cf9-216">Cuando salga del campo, verá un mensaje de error de validación del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="53cf9-216">When you tab out of the field, you see a client-side validation error message.</span></span>

![Ciudad necesario](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image14.png)

<span data-ttu-id="53cf9-218">De forma similar, para cada parámetro en la regla de validación del cliente, se agrega un atributo que tiene el formato `data-val-rulename-paramname=paramvalue`.</span><span class="sxs-lookup"><span data-stu-id="53cf9-218">Similarly, for each parameter in the client-validation rule, an attribute is added that has the form `data-val-rulename-paramname=paramvalue`.</span></span> <span data-ttu-id="53cf9-219">Por ejemplo, el `FirstName` propiedad se anota con el [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) atributo y especifica una longitud mínima de 3 y una longitud máxima de 8.</span><span class="sxs-lookup"><span data-stu-id="53cf9-219">For example, the `FirstName` property is annotated with the [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) attribute and specifies a minimum length of 3 and a maximum length of 8.</span></span> <span data-ttu-id="53cf9-220">La regla de validación de datos denominada `length` tiene el nombre del parámetro `max` y el valor del parámetro 8.</span><span class="sxs-lookup"><span data-stu-id="53cf9-220">The data validation rule named `length` has the parameter name `max` and the parameter value 8.</span></span> <span data-ttu-id="53cf9-221">La continuación muestra el código HTML que se genera para el `FirstName` campo cuando se modifica uno de los usuarios:</span><span class="sxs-lookup"><span data-stu-id="53cf9-221">The following shows the HTML that is generated for the `FirstName` field when you edit one of the users:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample16.cshtml)]

<span data-ttu-id="53cf9-222">Para obtener más información acerca de la validación del cliente discreta, vea la entrada [validación discreta de cliente en ASP.NET MVC 3](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html) en el blog de Brad Wilson.</span><span class="sxs-lookup"><span data-stu-id="53cf9-222">For more information about unobtrusive client validation, see the entry [Unobtrusive Client Validation in ASP.NET MVC 3](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html) in Brad Wilson's blog.</span></span>

> [!NOTE]
> <span data-ttu-id="53cf9-223">En la versión Beta de ASP.NET MVC 3, a veces es necesario enviar el formulario con el fin de iniciar la validación del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="53cf9-223">In ASP.NET MVC 3 Beta, you sometimes need to submit the form in order to start client-side validation.</span></span> <span data-ttu-id="53cf9-224">Esto podría cambiar en la versión final.</span><span class="sxs-lookup"><span data-stu-id="53cf9-224">This might be changed for the final release.</span></span>


## <a name="creating-the-create-view"></a><span data-ttu-id="53cf9-225">Creación de la vista de creación</span><span class="sxs-lookup"><span data-stu-id="53cf9-225">Creating the Create View</span></span>

<span data-ttu-id="53cf9-226">El siguiente paso es agregar un `Create` método de acción y vista para permitir que el usuario crear un nuevo usuario.</span><span class="sxs-lookup"><span data-stu-id="53cf9-226">The next step is to add a `Create` action method and view in order to enable the user to create a new user.</span></span> <span data-ttu-id="53cf9-227">Agregue las siguientes `Create` método al controlador home:</span><span class="sxs-lookup"><span data-stu-id="53cf9-227">Add the following `Create` method to the home controller:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample17.cs)]

<span data-ttu-id="53cf9-228">Agrega una vista como en los pasos anteriores, pero establece **ver contenido** a **crear**.</span><span class="sxs-lookup"><span data-stu-id="53cf9-228">Add a view as in the previous steps, but set **View content** to **Create**.</span></span>

![Crear vista](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image15.png)

<span data-ttu-id="53cf9-230">Ejecute la aplicación, seleccione la **crear** vincular, y agregar un nuevo usuario.</span><span class="sxs-lookup"><span data-stu-id="53cf9-230">Run the application, select the **Create** link, and add a new user.</span></span> <span data-ttu-id="53cf9-231">El `Create` método automáticamente aprovecha las ventajas de la validación del lado cliente y servidor.</span><span class="sxs-lookup"><span data-stu-id="53cf9-231">The `Create` method automatically takes advantage of client-side and server-side validation.</span></span> <span data-ttu-id="53cf9-232">Intenta escribir un nombre de usuario que contiene espacios en blanco, como &quot;Ben X&quot;.</span><span class="sxs-lookup"><span data-stu-id="53cf9-232">Try to enter a user name that contains white space, such as &quot;Ben X&quot;.</span></span> <span data-ttu-id="53cf9-233">Cuando salga del campo de nombre de usuario, un error de validación del lado cliente (`White space is not allowed`) se muestra.</span><span class="sxs-lookup"><span data-stu-id="53cf9-233">When you tab out of the user name field, a client-side validation error (`White space is not allowed`) is displayed.</span></span>

## <a name="add-the-delete-method"></a><span data-ttu-id="53cf9-234">Agregue el método Delete</span><span class="sxs-lookup"><span data-stu-id="53cf9-234">Add the Delete method</span></span>

<span data-ttu-id="53cf9-235">Para completar el tutorial, agregue las siguientes `Delete` método al controlador home:</span><span class="sxs-lookup"><span data-stu-id="53cf9-235">To complete the tutorial, add the following `Delete` method to the home controller:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample18.cs)]

<span data-ttu-id="53cf9-236">Agregar un `Delete` vista como en los pasos anteriores, establecer **ver contenido** a **eliminar**.</span><span class="sxs-lookup"><span data-stu-id="53cf9-236">Add a `Delete` view as in the previous steps, setting **View content** to **Delete**.</span></span>

![Eliminar vista](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image16.png)

<span data-ttu-id="53cf9-238">Ahora tiene una aplicación de ASP.NET MVC 3 simple pero totalmente funcional con la validación.</span><span class="sxs-lookup"><span data-stu-id="53cf9-238">You now have a simple but fully functional ASP.NET MVC 3 application with validation.</span></span>
