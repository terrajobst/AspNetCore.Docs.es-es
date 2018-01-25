---
title: "Crear una aplicación de ASP.NET Core con datos de usuario protegidos por autorización"
author: rick-anderson
ms.author: riande
manager: wpickett
ms.date: 05/22/2017
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: security/authorization/secure-data
ms.openlocfilehash: 7404b8ec20ed6a00554c8a7ade9a282362b9a186
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a><span data-ttu-id="a97c5-102">Crear una aplicación de ASP.NET Core con datos de usuario protegidos por autorización</span><span class="sxs-lookup"><span data-stu-id="a97c5-102">Create an ASP.NET Core app with user data protected by authorization</span></span>

<span data-ttu-id="a97c5-103">Por [Rick Anderson](https://twitter.com/RickAndMSFT) y [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="a97c5-103">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="a97c5-104">Este tutorial muestra cómo crear una aplicación web con datos de usuario protegidos mediante autorización.</span><span class="sxs-lookup"><span data-stu-id="a97c5-104">This tutorial shows how to create a web app with user data protected by authorization.</span></span> <span data-ttu-id="a97c5-105">Muestra una lista de contactos que autentica los usuarios (registrados) ha creado.</span><span class="sxs-lookup"><span data-stu-id="a97c5-105">It  displays a list of contacts that authenticated (registered) users have created.</span></span> <span data-ttu-id="a97c5-106">Hay tres grupos de seguridad:</span><span class="sxs-lookup"><span data-stu-id="a97c5-106">There are three security groups:</span></span>

* <span data-ttu-id="a97c5-107">Los usuarios registrados pueden ver todos los datos de contacto aprobados.</span><span class="sxs-lookup"><span data-stu-id="a97c5-107">Registered users can view all the approved contact data.</span></span>
* <span data-ttu-id="a97c5-108">Los usuarios registrados pueden editar o eliminar sus propios datos.</span><span class="sxs-lookup"><span data-stu-id="a97c5-108">Registered users can edit/delete their own data.</span></span> 
* <span data-ttu-id="a97c5-109">Los administradores pueden aprobar o rechazar datos de contacto.</span><span class="sxs-lookup"><span data-stu-id="a97c5-109">Managers can approve or reject contact data.</span></span> <span data-ttu-id="a97c5-110">Sólo los contactos aprobados son visibles para los usuarios.</span><span class="sxs-lookup"><span data-stu-id="a97c5-110">Only approved contacts are visible to users.</span></span>
* <span data-ttu-id="a97c5-111">Los administradores pueden aprobar o rechazar y editar o eliminar los datos.</span><span class="sxs-lookup"><span data-stu-id="a97c5-111">Administrators can approve/reject and edit/delete any data.</span></span>

<span data-ttu-id="a97c5-112">En la siguiente imagen, usuario Rick (`rick@example.com`) ha iniciado sesión.</span><span class="sxs-lookup"><span data-stu-id="a97c5-112">In the following image, user Rick (`rick@example.com`) is signed in.</span></span> <span data-ttu-id="a97c5-113">Usuario Rick solo pueden ver los contactos aprobados y editar o eliminar sus contactos.</span><span class="sxs-lookup"><span data-stu-id="a97c5-113">User Rick can only view approved contacts and edit/delete his contacts.</span></span> <span data-ttu-id="a97c5-114">El último registro, creado por Rick, muestra Editar y eliminar los vínculos</span><span class="sxs-lookup"><span data-stu-id="a97c5-114">Only the last record, created by Rick, displays edit and delete links</span></span>

![imagen que se ha descrito anteriormente](secure-data/_static/rick.png)

<span data-ttu-id="a97c5-116">En la siguiente imagen, `manager@contoso.com` está firmado en y en el rol de administrador.</span><span class="sxs-lookup"><span data-stu-id="a97c5-116">In the following image, `manager@contoso.com` is signed in and in the managers role.</span></span> 

![imagen que se ha descrito anteriormente](secure-data/_static/manager1.png)

<span data-ttu-id="a97c5-118">La siguiente imagen muestra a los administradores de la vista de detalles de un contacto.</span><span class="sxs-lookup"><span data-stu-id="a97c5-118">The following image shows the  managers details view of a contact.</span></span>

![imagen que se ha descrito anteriormente](secure-data/_static/manager.png)

<span data-ttu-id="a97c5-120">Sólo los administradores y los administradores tienen la aprobar y rechazan botones.</span><span class="sxs-lookup"><span data-stu-id="a97c5-120">Only managers and administrators have the approve and reject buttons.</span></span>

<span data-ttu-id="a97c5-121">En la siguiente imagen, `admin@contoso.com` está firmado en y en el rol del administrador.</span><span class="sxs-lookup"><span data-stu-id="a97c5-121">In the following image, `admin@contoso.com` is signed in and in the administrator’s role.</span></span> 

![imagen que se ha descrito anteriormente](secure-data/_static/admin.png)

<span data-ttu-id="a97c5-123">El administrador tiene todos los privilegios.</span><span class="sxs-lookup"><span data-stu-id="a97c5-123">The administrator has all privileges.</span></span> <span data-ttu-id="a97c5-124">Puede leer, editar o eliminar cualquier contacto y cambiar el estado de contactos.</span><span class="sxs-lookup"><span data-stu-id="a97c5-124">She can read/edit/delete any contact and change the status of contacts.</span></span>

<span data-ttu-id="a97c5-125">La aplicación se creó con [scaffolding](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller) siguiente `Contact` modelo:</span><span class="sxs-lookup"><span data-stu-id="a97c5-125">The app was created by [scaffolding](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller)  the following `Contact` model:</span></span>

[!code-csharp[Main](secure-data/samples/starter/Models/Contact.cs?name=snippet1)]

<span data-ttu-id="a97c5-126">Un `ContactIsOwnerAuthorizationHandler` controlador autorización garantiza que un usuario solo puede editar sus datos.</span><span class="sxs-lookup"><span data-stu-id="a97c5-126">A `ContactIsOwnerAuthorizationHandler` authorization handler ensures that a user can only edit their data.</span></span> <span data-ttu-id="a97c5-127">Un `ContactManagerAuthorizationHandler` controlador de autorización permite a los administradores aprobar o rechazar los contactos.</span><span class="sxs-lookup"><span data-stu-id="a97c5-127">A `ContactManagerAuthorizationHandler` authorization handler allows managers to approve or reject contacts.</span></span>  <span data-ttu-id="a97c5-128">Un `ContactAdministratorsAuthorizationHandler` controlador de autorización permite a los administradores para aprobar o rechazar los contactos y editar o eliminar contactos.</span><span class="sxs-lookup"><span data-stu-id="a97c5-128">A `ContactAdministratorsAuthorizationHandler` authorization handler allows administrators to approve or reject contacts and to edit/delete contacts.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="a97c5-129">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="a97c5-129">Prerequisites</span></span>

<span data-ttu-id="a97c5-130">Esto no es un tutorial de inicio.</span><span class="sxs-lookup"><span data-stu-id="a97c5-130">This isn't a beginning tutorial.</span></span> <span data-ttu-id="a97c5-131">Debe estar familiarizado con:</span><span class="sxs-lookup"><span data-stu-id="a97c5-131">You should be familiar with:</span></span>

* [<span data-ttu-id="a97c5-132">Núcleo de ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="a97c5-132">ASP.NET Core MVC</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="a97c5-133">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="a97c5-133">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

## <a name="the-starter-and-completed-app"></a><span data-ttu-id="a97c5-134">El inicio y la aplicación completada</span><span class="sxs-lookup"><span data-stu-id="a97c5-134">The starter and completed app</span></span>

<span data-ttu-id="a97c5-135">[Descargar](xref:tutorials/index#how-to-download-a-sample) el [completado](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final) aplicación.</span><span class="sxs-lookup"><span data-stu-id="a97c5-135">[Download](xref:tutorials/index#how-to-download-a-sample) the [completed](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final) app.</span></span> <span data-ttu-id="a97c5-136">[Prueba](#test-the-completed-app) la aplicación completada por lo que se familiarice con sus características de seguridad.</span><span class="sxs-lookup"><span data-stu-id="a97c5-136">[Test](#test-the-completed-app) the completed app so you become familiar with its security features.</span></span> 

### <a name="the-starter-app"></a><span data-ttu-id="a97c5-137">La aplicación de inicio</span><span class="sxs-lookup"><span data-stu-id="a97c5-137">The starter app</span></span>

<span data-ttu-id="a97c5-138">Es útil comparar el código con el ejemplo completo.</span><span class="sxs-lookup"><span data-stu-id="a97c5-138">It's helpful to compare your code with the completed sample.</span></span>

<span data-ttu-id="a97c5-139">[Descargar](xref:tutorials/index#how-to-download-a-sample) el [starter](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter) aplicación.</span><span class="sxs-lookup"><span data-stu-id="a97c5-139">[Download](xref:tutorials/index#how-to-download-a-sample) the [starter](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter) app.</span></span> 

<span data-ttu-id="a97c5-140">Vea [crear la aplicación de inicio](#create-the-starter-app) si le gustaría crear desde cero.</span><span class="sxs-lookup"><span data-stu-id="a97c5-140">See [Create the starter app](#create-the-starter-app) if you'd like to create it from scratch.</span></span>

<span data-ttu-id="a97c5-141">Actualizar la base de datos:</span><span class="sxs-lookup"><span data-stu-id="a97c5-141">Update the database:</span></span>

```none
   dotnet ef database update
```

<span data-ttu-id="a97c5-142">Ejecutar la aplicación, pulse el **ContactManager** vincular y compruebe que puede crear, editar y eliminar un contacto.</span><span class="sxs-lookup"><span data-stu-id="a97c5-142">Run the app, tap the **ContactManager** link, and verify you can create, edit, and delete a contact.</span></span>

<span data-ttu-id="a97c5-143">Este tutorial tiene todos los pasos principales para crear la aplicación de datos de usuario seguras.</span><span class="sxs-lookup"><span data-stu-id="a97c5-143">This tutorial has all the major steps to create the secure user data app.</span></span> <span data-ttu-id="a97c5-144">Le resultará útil para hacer referencia al proyecto completado.</span><span class="sxs-lookup"><span data-stu-id="a97c5-144">You may find it helpful to refer to the completed project.</span></span>

## <a name="modify-the-app-to-secure-user-data"></a><span data-ttu-id="a97c5-145">Modificar la aplicación para proteger los datos de usuario</span><span class="sxs-lookup"><span data-stu-id="a97c5-145">Modify the app to secure user data</span></span>

<span data-ttu-id="a97c5-146">Las siguientes secciones contienen todos los pasos principales para crear la aplicación de datos de usuario seguras.</span><span class="sxs-lookup"><span data-stu-id="a97c5-146">The following sections have all the major steps to create the secure user data app.</span></span> <span data-ttu-id="a97c5-147">Le resultará útil para hacer referencia al proyecto completado.</span><span class="sxs-lookup"><span data-stu-id="a97c5-147">You may find it helpful to refer to the completed project.</span></span>

### <a name="tie-the-contact-data-to-the-user"></a><span data-ttu-id="a97c5-148">Asociar los datos de contacto para el usuario</span><span class="sxs-lookup"><span data-stu-id="a97c5-148">Tie the contact data to the user</span></span>

<span data-ttu-id="a97c5-149">Usar ASP.NET [identidad](xref:security/authentication/identity) Id. de usuario para garantizar que los usuarios puede editar sus datos, pero no otros datos de los usuarios.</span><span class="sxs-lookup"><span data-stu-id="a97c5-149">Use the ASP.NET [Identity](xref:security/authentication/identity) user ID to ensure users can edit their data, but not other users data.</span></span> <span data-ttu-id="a97c5-150">Agregar `OwnerID` a la `Contact` modelo:</span><span class="sxs-lookup"><span data-stu-id="a97c5-150">Add `OwnerID` to the `Contact` model:</span></span>

[!code-csharp[Main](secure-data/samples/final/Models/Contact.cs?name=snippet1&highlight=5-6,16-)]

<span data-ttu-id="a97c5-151">`OwnerID`es el identificador del usuario desde el `AspNetUser` tabla el [identidad](xref:security/authentication/identity) base de datos.</span><span class="sxs-lookup"><span data-stu-id="a97c5-151">`OwnerID` is the user's ID from the `AspNetUser` table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="a97c5-152">El `Status` campo determina si un contacto es visible para los usuarios en general.</span><span class="sxs-lookup"><span data-stu-id="a97c5-152">The `Status` field determines if a contact is viewable by general users.</span></span> 

<span data-ttu-id="a97c5-153">Aplicar la técnica scaffolding una nueva migración y actualizar la base de datos:</span><span class="sxs-lookup"><span data-stu-id="a97c5-153">Scaffold a new migration and update the database:</span></span>

```console
dotnet ef migrations add userID_Status
dotnet ef database update
 ```

### <a name="require-ssl-and-authenticated-users"></a><span data-ttu-id="a97c5-154">Requerir SSL y los usuarios autenticados</span><span class="sxs-lookup"><span data-stu-id="a97c5-154">Require SSL and authenticated users</span></span>

<span data-ttu-id="a97c5-155">En el `ConfigureServices` método de la *Startup.cs* , agregue el [RequireHttpsAttribute](/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) filtro de autorización:</span><span class="sxs-lookup"><span data-stu-id="a97c5-155">In the `ConfigureServices` method of the *Startup.cs* file, add the [RequireHttpsAttribute](/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) authorization filter:</span></span>

[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=snippet_SSL&highlight=1)]

<span data-ttu-id="a97c5-156">Para redirigir las solicitudes HTTP a HTTPS, consulte [Middleware de reescritura de dirección URL](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="a97c5-156">To redirect HTTP requests to HTTPS, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span> <span data-ttu-id="a97c5-157">Si está utilizando Visual Studio Code o pruebas en plataforma local no incluye un certificado de prueba para SSL:</span><span class="sxs-lookup"><span data-stu-id="a97c5-157">If you are using Visual Studio Code or testing on local platform that doesn't include a test certificate for SSL:</span></span>

- <span data-ttu-id="a97c5-158">Establecer `"LocalTest:skipSSL": true` en el *appSettings.JSON que se* archivo.</span><span class="sxs-lookup"><span data-stu-id="a97c5-158">Set `"LocalTest:skipSSL": true` in the *appsettings.json* file.</span></span>

### <a name="require-authenticated-users"></a><span data-ttu-id="a97c5-159">Requerir a los usuarios autenticados</span><span class="sxs-lookup"><span data-stu-id="a97c5-159">Require authenticated users</span></span>

<span data-ttu-id="a97c5-160">Establecer la directiva de autenticación predeterminado para exigir que los usuarios se autentiquen.</span><span class="sxs-lookup"><span data-stu-id="a97c5-160">Set the default authentication policy to require users to be authenticated.</span></span> <span data-ttu-id="a97c5-161">Puede rechazar la autenticación en el método de acción o controlador con el `[AllowAnonymous]` atributo.</span><span class="sxs-lookup"><span data-stu-id="a97c5-161">You can opt out of authentication at the controller or action method with the `[AllowAnonymous]` attribute.</span></span> <span data-ttu-id="a97c5-162">Con este enfoque, los nuevos controladores que se agrega automáticamente requerirá la autenticación, que es más segura que confiar en los nuevos controladores para incluir la `[Authorize]` atributo.</span><span class="sxs-lookup"><span data-stu-id="a97c5-162">With this approach, any new controllers added will automatically require authentication, which is safer than relying on new controllers to include the `[Authorize]` attribute.</span></span> <span data-ttu-id="a97c5-163">Agregue lo siguiente a la `ConfigureServices` método de la *Startup.cs* archivo:</span><span class="sxs-lookup"><span data-stu-id="a97c5-163">Add the following to  the `ConfigureServices` method of the *Startup.cs* file:</span></span>

[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=snippet_defaultPolicy)]

<span data-ttu-id="a97c5-164">Agregar `[AllowAnonymous]` al controlador home para los usuarios anónimos pueden recibir información sobre el sitio antes de que registrar.</span><span class="sxs-lookup"><span data-stu-id="a97c5-164">Add `[AllowAnonymous]` to the home controller so anonymous users can get information about the site before they register.</span></span>

[!code-csharp[Main](secure-data/samples/final/Controllers/HomeController.cs?name=snippet1&highlight=2,6)]

### <a name="configure-the-test-account"></a><span data-ttu-id="a97c5-165">Configurar la cuenta de prueba</span><span class="sxs-lookup"><span data-stu-id="a97c5-165">Configure the test account</span></span>

<span data-ttu-id="a97c5-166">La `SeedData` clase crea dos cuentas, el administrador y el administrador.</span><span class="sxs-lookup"><span data-stu-id="a97c5-166">The `SeedData` class creates two accounts,  administrator and manager.</span></span> <span data-ttu-id="a97c5-167">Use la [herramienta secreto administrador](xref:security/app-secrets) para establecer una contraseña para estas cuentas.</span><span class="sxs-lookup"><span data-stu-id="a97c5-167">Use the [Secret Manager tool](xref:security/app-secrets) to set a password for these accounts.</span></span> <span data-ttu-id="a97c5-168">Desde el directorio del proyecto (el directorio que contiene *Program.cs*).</span><span class="sxs-lookup"><span data-stu-id="a97c5-168">Do this from the project directory (the directory containing *Program.cs*).</span></span>

```console
dotnet user-secrets set SeedUserPW <PW>
```

<span data-ttu-id="a97c5-169">Actualización `Configure` usar la contraseña de prueba:</span><span class="sxs-lookup"><span data-stu-id="a97c5-169">Update `Configure` to use the test password:</span></span>

[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=Configure&highlight=19-21)]

<span data-ttu-id="a97c5-170">Agregue el identificador de usuario de administrador y `Status = ContactStatus.Approved` a los contactos.</span><span class="sxs-lookup"><span data-stu-id="a97c5-170">Add the administrator user ID and `Status = ContactStatus.Approved` to the contacts.</span></span> <span data-ttu-id="a97c5-171">Se muestra sólo un contacto, agregue el identificador de usuario para todos los contactos:</span><span class="sxs-lookup"><span data-stu-id="a97c5-171">Only one contact is shown, add the user ID to all contacts:</span></span>

[!code-csharp[Main](secure-data/samples/final/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a><span data-ttu-id="a97c5-172">Crear propietario, el administrador y los controladores de autorización de administrador</span><span class="sxs-lookup"><span data-stu-id="a97c5-172">Create owner, manager, and administrator authorization handlers</span></span>

<span data-ttu-id="a97c5-173">Crear un `ContactIsOwnerAuthorizationHandler` clase en el *autorización* carpeta.</span><span class="sxs-lookup"><span data-stu-id="a97c5-173">Create a `ContactIsOwnerAuthorizationHandler` class in the  *Authorization* folder.</span></span> <span data-ttu-id="a97c5-174">El `ContactIsOwnerAuthorizationHandler` comprobará que el usuario que actúa en el recurso es propietario del recurso.</span><span class="sxs-lookup"><span data-stu-id="a97c5-174">The `ContactIsOwnerAuthorizationHandler` will verify the user acting on the resource owns the resource.</span></span>

[!code-csharp[Main](secure-data/samples/final/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

<span data-ttu-id="a97c5-175">El `ContactIsOwnerAuthorizationHandler` llamadas `context.Succeed` si el usuario autenticado actual es el propietario del contacto.</span><span class="sxs-lookup"><span data-stu-id="a97c5-175">The `ContactIsOwnerAuthorizationHandler` calls `context.Succeed` if the current authenticated user is the contact owner.</span></span> <span data-ttu-id="a97c5-176">Controladores de autorización generalmente devuelven `context.Succeed` cuando se cumplen los requisitos.</span><span class="sxs-lookup"><span data-stu-id="a97c5-176">Authorization handlers generally return `context.Succeed` when the requirements are met.</span></span> <span data-ttu-id="a97c5-177">Devuelven `Task.FromResult(0)` cuando no se cumplen los requisitos.</span><span class="sxs-lookup"><span data-stu-id="a97c5-177">They return `Task.FromResult(0)` when requirements are not met.</span></span> <span data-ttu-id="a97c5-178">`Task.FromResult(0)`no correcto o error, permite que otro controlador de autorización para que se ejecute.</span><span class="sxs-lookup"><span data-stu-id="a97c5-178">`Task.FromResult(0)` is neither success or failure, it allows other authorization handler to run.</span></span> <span data-ttu-id="a97c5-179">Si necesita explícitamente un error, devuelve `context.Fail()`.</span><span class="sxs-lookup"><span data-stu-id="a97c5-179">If you need to explicitly fail, return `context.Fail()`.</span></span>

<span data-ttu-id="a97c5-180">Se permiten los propietarios de contacto editar o eliminar sus propios datos, por lo que no es necesario comprobar la operación pasada en el parámetro de requisito.</span><span class="sxs-lookup"><span data-stu-id="a97c5-180">We allow contact owners to edit/delete their own data, so we don't need to check the operation passed in the requirement parameter.</span></span>

### <a name="create-a-manager-authorization-handler"></a><span data-ttu-id="a97c5-181">Crear un controlador del Administrador de autorización</span><span class="sxs-lookup"><span data-stu-id="a97c5-181">Create a manager authorization handler</span></span>

<span data-ttu-id="a97c5-182">Crear un `ContactManagerAuthorizationHandler` clase en el *autorización* carpeta.</span><span class="sxs-lookup"><span data-stu-id="a97c5-182">Create a `ContactManagerAuthorizationHandler` class in the  *Authorization* folder.</span></span> <span data-ttu-id="a97c5-183">El `ContactManagerAuthorizationHandler` comprobará que el usuario que actúa en el recurso es un administrador.</span><span class="sxs-lookup"><span data-stu-id="a97c5-183">The `ContactManagerAuthorizationHandler` will verify the user acting on the resource is a manager.</span></span> <span data-ttu-id="a97c5-184">Solo los administradores pueden aprobar o rechazar los cambios de contenido (nuevos o modificados).</span><span class="sxs-lookup"><span data-stu-id="a97c5-184">Only managers can approve or reject content changes (new or changed).</span></span>

[!code-csharp[Main](secure-data/samples/final/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a><span data-ttu-id="a97c5-185">Crear un controlador de autorización de administrador</span><span class="sxs-lookup"><span data-stu-id="a97c5-185">Create an administrator authorization handler</span></span>

<span data-ttu-id="a97c5-186">Crear un `ContactAdministratorsAuthorizationHandler` clase en el *autorización* carpeta.</span><span class="sxs-lookup"><span data-stu-id="a97c5-186">Create a `ContactAdministratorsAuthorizationHandler` class in the  *Authorization* folder.</span></span> <span data-ttu-id="a97c5-187">El `ContactAdministratorsAuthorizationHandler` comprobará que el usuario que actúa en el recurso es un administrador.</span><span class="sxs-lookup"><span data-stu-id="a97c5-187">The `ContactAdministratorsAuthorizationHandler` will verify the user acting on the resource is a administrator.</span></span> <span data-ttu-id="a97c5-188">Administrador puede hacer que todas las operaciones.</span><span class="sxs-lookup"><span data-stu-id="a97c5-188">Administrator can do all operations.</span></span>

[!code-csharp[Main](secure-data/samples/final/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a><span data-ttu-id="a97c5-189">Registrar los controladores de autorización</span><span class="sxs-lookup"><span data-stu-id="a97c5-189">Register the authorization handlers</span></span>

<span data-ttu-id="a97c5-190">Servicios mediante Entity Framework Core deben estar registrados para [inyección de dependencia](xref:fundamentals/dependency-injection) con [AddScoped](/aspnet/core/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span><span class="sxs-lookup"><span data-stu-id="a97c5-190">Services using Entity Framework Core must be registered for [dependency injection](xref:fundamentals/dependency-injection) using [AddScoped](/aspnet/core/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span></span> <span data-ttu-id="a97c5-191">El `ContactIsOwnerAuthorizationHandler` usa ASP.NET Core [identidad](xref:security/authentication/identity), que se basa en Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="a97c5-191">The `ContactIsOwnerAuthorizationHandler` uses ASP.NET Core [Identity](xref:security/authentication/identity), which is built on Entity Framework Core.</span></span> <span data-ttu-id="a97c5-192">Registre los controladores con la colección de servicio para que estén disponibles para la `ContactsController` a través de [inyección de dependencia](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="a97c5-192">Register the handlers with the service collection so they will be available to the `ContactsController` through [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="a97c5-193">Agregue el código siguiente al final de `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="a97c5-193">Add the following code to the end of `ConfigureServices`:</span></span>

[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=AuthorizationHandlers)]

<span data-ttu-id="a97c5-194">`ContactAdministratorsAuthorizationHandler`y `ContactManagerAuthorizationHandler` se agregan como singleton.</span><span class="sxs-lookup"><span data-stu-id="a97c5-194">`ContactAdministratorsAuthorizationHandler` and `ContactManagerAuthorizationHandler` are added as singletons.</span></span> <span data-ttu-id="a97c5-195">Son singletons porque no usan EF y toda la información necesaria se encuentra en la `Context` parámetro de la `HandleRequirementAsync` método.</span><span class="sxs-lookup"><span data-stu-id="a97c5-195">They're singletons because they don't use EF and all the information needed is in the `Context` parameter of the `HandleRequirementAsync` method.</span></span>

<span data-ttu-id="a97c5-196">La sección completa `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="a97c5-196">The complete `ConfigureServices`:</span></span>

[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=ConfigureServices)]

## <a name="update-the-code-to-support-authorization"></a><span data-ttu-id="a97c5-197">Actualizar el código para admitir la autorización</span><span class="sxs-lookup"><span data-stu-id="a97c5-197">Update the code to support authorization</span></span>

<span data-ttu-id="a97c5-198">En esta sección, actualice el controlador y vistas y agregar una clase de requisitos de las operaciones.</span><span class="sxs-lookup"><span data-stu-id="a97c5-198">In this section, you update the controller and views and add an operations requirements class.</span></span>

### <a name="update-the-contacts-controller"></a><span data-ttu-id="a97c5-199">Actualice el controlador de contactos</span><span class="sxs-lookup"><span data-stu-id="a97c5-199">Update the Contacts controller</span></span>

<span data-ttu-id="a97c5-200">Actualización de la `ContactsController` constructor:</span><span class="sxs-lookup"><span data-stu-id="a97c5-200">Update the `ContactsController` constructor:</span></span>

* <span data-ttu-id="a97c5-201">Agregar el `IAuthorizationService` servicio para tener acceso a los controladores de autorización.</span><span class="sxs-lookup"><span data-stu-id="a97c5-201">Add the `IAuthorizationService` service to  access to the authorization handlers.</span></span> 
* <span data-ttu-id="a97c5-202">Agregar el `Identity` `UserManager` servicio:</span><span class="sxs-lookup"><span data-stu-id="a97c5-202">Add the `Identity` `UserManager` service:</span></span>

[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_ContactsControllerCtor)]

### <a name="add-a-contact-operations-requirements-class"></a><span data-ttu-id="a97c5-203">Agregar una clase de requisitos de operaciones de contacto</span><span class="sxs-lookup"><span data-stu-id="a97c5-203">Add a contact operations requirements class</span></span>

<span data-ttu-id="a97c5-204">Agregar el `ContactOperations` clase a la *autorización* carpeta.</span><span class="sxs-lookup"><span data-stu-id="a97c5-204">Add the `ContactOperations` class to the *Authorization* folder.</span></span> <span data-ttu-id="a97c5-205">Esta clase contiene los requisitos de la aplicación admite:</span><span class="sxs-lookup"><span data-stu-id="a97c5-205">This class  contain the requirements our app supports:</span></span>

[!code-csharp[Main](secure-data/samples/final/Authorization/ContactOperations.cs)]

### <a name="update-create"></a><span data-ttu-id="a97c5-206">Crear actualización</span><span class="sxs-lookup"><span data-stu-id="a97c5-206">Update Create</span></span>

<span data-ttu-id="a97c5-207">Actualización de la `HTTP POST Create` método:</span><span class="sxs-lookup"><span data-stu-id="a97c5-207">Update the `HTTP POST Create` method to:</span></span>

* <span data-ttu-id="a97c5-208">Agregar el identificador de usuario para el `Contact` modelo.</span><span class="sxs-lookup"><span data-stu-id="a97c5-208">Add the user ID to the `Contact` model.</span></span>
* <span data-ttu-id="a97c5-209">Llamar al controlador de autorización para comprobar que el usuario es propietario del contacto.</span><span class="sxs-lookup"><span data-stu-id="a97c5-209">Call the authorization handler to verify the user owns the contact.</span></span>

[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_Create)]

### <a name="update-edit"></a><span data-ttu-id="a97c5-210">Edición de actualización</span><span class="sxs-lookup"><span data-stu-id="a97c5-210">Update Edit</span></span>

<span data-ttu-id="a97c5-211">Actualizarlas `Edit` métodos para usar el controlador de autorización para comprobar que el usuario propietario del contacto.</span><span class="sxs-lookup"><span data-stu-id="a97c5-211">Update both `Edit` methods to use the authorization handler to verify the user owns the contact.</span></span> <span data-ttu-id="a97c5-212">Dado que se está realizando la autorización de recurso no podemos usar la `[Authorize]` atributo.</span><span class="sxs-lookup"><span data-stu-id="a97c5-212">Because we are performing resource authorization we cannot use the `[Authorize]` attribute.</span></span> <span data-ttu-id="a97c5-213">No tenemos acceso a los recursos cuando se evalúan los atributos.</span><span class="sxs-lookup"><span data-stu-id="a97c5-213">We don't have access to the resource when attributes are evaluated.</span></span> <span data-ttu-id="a97c5-214">Autorización basada en recursos debe ser imperativo.</span><span class="sxs-lookup"><span data-stu-id="a97c5-214">Resource based authorization must be imperative.</span></span> <span data-ttu-id="a97c5-215">Las comprobaciones deben realizarse una vez que tenemos acceso a los recursos, cargándolo en un control o mediante la carga dentro de su propio controlador.</span><span class="sxs-lookup"><span data-stu-id="a97c5-215">Checks must be performed once we have access to the resource, either by loading it in our controller, or by loading it within the handler itself.</span></span> <span data-ttu-id="a97c5-216">Con frecuencia tendrán acceso al recurso pasando la clave de recurso.</span><span class="sxs-lookup"><span data-stu-id="a97c5-216">Frequently you will access the resource by passing in the resource key.</span></span>

[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_Edit)]

### <a name="update-the-delete-method"></a><span data-ttu-id="a97c5-217">El método Delete de actualización</span><span class="sxs-lookup"><span data-stu-id="a97c5-217">Update the Delete method</span></span>

<span data-ttu-id="a97c5-218">Actualizarlas `Delete` métodos para usar el controlador de autorización para comprobar que el usuario propietario del contacto.</span><span class="sxs-lookup"><span data-stu-id="a97c5-218">Update both `Delete` methods to use the authorization handler to verify the user owns the contact.</span></span>

[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_Delete)]

## <a name="inject-the-authorization-service-into-the-views"></a><span data-ttu-id="a97c5-219">Insertar el servicio de autorización en las vistas</span><span class="sxs-lookup"><span data-stu-id="a97c5-219">Inject the authorization service into the views</span></span>

<span data-ttu-id="a97c5-220">Actualmente se muestra en la interfaz de usuario edita y elimina los vínculos de datos que no se puede modificar el usuario.</span><span class="sxs-lookup"><span data-stu-id="a97c5-220">Currently the UI shows edit and delete links for data the user cannot modify.</span></span> <span data-ttu-id="a97c5-221">Se corregirá aplicando el controlador de autorización a las vistas.</span><span class="sxs-lookup"><span data-stu-id="a97c5-221">We'll fix that by applying the authorization handler to the views.</span></span>

<span data-ttu-id="a97c5-222">Insertar el servicio de autorización en el *Views/_ViewImports.cshtml* por lo que estará disponible para todas las vistas de archivos:</span><span class="sxs-lookup"><span data-stu-id="a97c5-222">Inject the authorization service in the *Views/_ViewImports.cshtml* file so it will be available to all views:</span></span>

[!code-html[Main](secure-data/samples/final/Views/_ViewImports.cshtml)]

<span data-ttu-id="a97c5-223">Actualización de la *Views/Contacts/Index.cshtml* vista Razor para mostrar la edición solo y eliminar los vínculos para los usuarios que pueden el contacto de editar o eliminar.</span><span class="sxs-lookup"><span data-stu-id="a97c5-223">Update the *Views/Contacts/Index.cshtml* Razor view to only display the edit and delete links for users who can edit/delete the contact.</span></span>

<span data-ttu-id="a97c5-224">Agregar`@using ContactManager.Authorization;`</span><span class="sxs-lookup"><span data-stu-id="a97c5-224">Add `@using ContactManager.Authorization;`</span></span>

<span data-ttu-id="a97c5-225">Actualización de la `Edit` y `Delete` vínculos para que solo se procesan para que los usuarios con permiso Modificar y eliminar el contacto.</span><span class="sxs-lookup"><span data-stu-id="a97c5-225">Update the `Edit` and `Delete` links so they're only rendered for users with permission to edit and delete the contact.</span></span>

[!code-html[Main](secure-data/samples/final/Views/Contacts/Index.cshtml?range=63-84)]

<span data-ttu-id="a97c5-226">Advertencia: Ocultar los vínculos de los usuarios que no tienen permiso para editar o eliminar los datos no proteger la aplicación.</span><span class="sxs-lookup"><span data-stu-id="a97c5-226">Warning: Hiding links from users that don't have permission to edit or delete data doesn't secure the app.</span></span> <span data-ttu-id="a97c5-227">Ocultar los vínculos hace que la aplicación de usuario más descriptivo mostrando vínculos solo es válido.</span><span class="sxs-lookup"><span data-stu-id="a97c5-227">Hiding links makes the app more user friendly by displaying only valid links.</span></span> <span data-ttu-id="a97c5-228">Los usuarios pueden hack las direcciones URL generadas para invocar Editar y eliminar operaciones en los datos que no poseen.</span><span class="sxs-lookup"><span data-stu-id="a97c5-228">Users can hack the generated URLs to invoke edit and delete operations on data they don't own.</span></span>  <span data-ttu-id="a97c5-229">El controlador debe repetir que comprueba el acceso para que sea seguro.</span><span class="sxs-lookup"><span data-stu-id="a97c5-229">The controller must repeat the access checks to be secure.</span></span>

### <a name="update-the-details-view"></a><span data-ttu-id="a97c5-230">Actualizar la vista de detalles</span><span class="sxs-lookup"><span data-stu-id="a97c5-230">Update the Details view</span></span>

<span data-ttu-id="a97c5-231">Actualice la vista de detalles para que los administradores pueden aprobar o rechazar contactos:</span><span class="sxs-lookup"><span data-stu-id="a97c5-231">Update the details view so managers can approve or reject contacts:</span></span>

[!code-html[Main](secure-data/samples/final/Views/Contacts/Details.cshtml?range=53-)]

## <a name="test-the-completed-app"></a><span data-ttu-id="a97c5-232">Probar la aplicación completada</span><span class="sxs-lookup"><span data-stu-id="a97c5-232">Test the completed app</span></span>

<span data-ttu-id="a97c5-233">Si está utilizando Visual Studio Code o pruebas en plataforma local no incluye un certificado de prueba para SSL:</span><span class="sxs-lookup"><span data-stu-id="a97c5-233">If you are using Visual Studio Code or testing on local platform that doesn't include a test certificate for SSL:</span></span>

- <span data-ttu-id="a97c5-234">Establecer `"LocalTest:skipSSL": true` en el *appSettings.JSON que se* archivo.</span><span class="sxs-lookup"><span data-stu-id="a97c5-234">Set `"LocalTest:skipSSL": true` in the *appsettings.json* file.</span></span>

<span data-ttu-id="a97c5-235">Si se ha ejecutado la aplicación y tener contactos, elimine todos los registros en el `Contact` de tabla y reinicie la aplicación para inicializar la base de datos.</span><span class="sxs-lookup"><span data-stu-id="a97c5-235">If you have run the app and have contacts, delete all the records in the `Contact` table and restart the app to seed the database.</span></span> <span data-ttu-id="a97c5-236">Si se utiliza Visual Studio, debe salir y reiniciar IIS Express al valor de inicialización de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="a97c5-236">If you are using Visual Studio, you need to exit and restart IIS Express to seed the database.</span></span>

<span data-ttu-id="a97c5-237">Registrar un usuario para examinar los contactos.</span><span class="sxs-lookup"><span data-stu-id="a97c5-237">Register a user to browse the contacts.</span></span>

<span data-ttu-id="a97c5-238">Una manera sencilla de probar la aplicación final consiste en iniciar tres distintos exploradores (o versiones de incógnito/InPrivate).</span><span class="sxs-lookup"><span data-stu-id="a97c5-238">An easy way to test the completed app is to launch three different browsers (or incognito/InPrivate versions).</span></span> <span data-ttu-id="a97c5-239">En un explorador, registrar un nuevo usuario, por ejemplo, `test@example.com`.</span><span class="sxs-lookup"><span data-stu-id="a97c5-239">In one browser, register a new user, for example, `test@example.com`.</span></span> <span data-ttu-id="a97c5-240">Inicie sesión con un usuario diferente en cada explorador.</span><span class="sxs-lookup"><span data-stu-id="a97c5-240">Sign in to each browser with a different user.</span></span> <span data-ttu-id="a97c5-241">Compruebe lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="a97c5-241">Verify the following:</span></span>

* <span data-ttu-id="a97c5-242">Los usuarios registrados pueden ver todos los datos de contacto aprobados.</span><span class="sxs-lookup"><span data-stu-id="a97c5-242">Registered users can view all the approved contact data.</span></span>
* <span data-ttu-id="a97c5-243">Los usuarios registrados pueden editar o eliminar sus propios datos.</span><span class="sxs-lookup"><span data-stu-id="a97c5-243">Registered users can edit/delete their own data.</span></span> 
* <span data-ttu-id="a97c5-244">Los administradores pueden aprobar o rechazar datos de contacto.</span><span class="sxs-lookup"><span data-stu-id="a97c5-244">Managers can approve or reject contact data.</span></span> <span data-ttu-id="a97c5-245">El `Details` vista muestra **aprobar** y **rechazar** botones.</span><span class="sxs-lookup"><span data-stu-id="a97c5-245">The `Details` view shows **Approve** and **Reject** buttons.</span></span> 
* <span data-ttu-id="a97c5-246">Los administradores pueden aprobar o rechazar y editar o eliminar los datos.</span><span class="sxs-lookup"><span data-stu-id="a97c5-246">Administrators can approve/reject and edit/delete any data.</span></span>

| <span data-ttu-id="a97c5-247">Usuario</span><span class="sxs-lookup"><span data-stu-id="a97c5-247">User</span></span>| <span data-ttu-id="a97c5-248">Opciones</span><span class="sxs-lookup"><span data-stu-id="a97c5-248">Options</span></span> |
| ------------ | ---------|
| test@example.com | <span data-ttu-id="a97c5-249">Editar puede/eliminar datos propios</span><span class="sxs-lookup"><span data-stu-id="a97c5-249">Can edit/delete own data</span></span> |
| manager@contoso.com | <span data-ttu-id="a97c5-250">Pueden aprobar o rechazar y editar o eliminar datos de propietario</span><span class="sxs-lookup"><span data-stu-id="a97c5-250">Can approve/reject and edit/delete own data</span></span>  |
| admin@contoso.com | <span data-ttu-id="a97c5-251">Puede editar o eliminar y aprobar o rechazar todos los datos</span><span class="sxs-lookup"><span data-stu-id="a97c5-251">Can edit/delete and approve/reject all data</span></span>|

<span data-ttu-id="a97c5-252">Crear un contacto en el Explorador de administradores.</span><span class="sxs-lookup"><span data-stu-id="a97c5-252">Create a contact in the administrators browser.</span></span> <span data-ttu-id="a97c5-253">Copie la dirección URL para eliminar y editar en el contacto del administrador.</span><span class="sxs-lookup"><span data-stu-id="a97c5-253">Copy the URL for delete and edit from the administrator contact.</span></span> <span data-ttu-id="a97c5-254">Pegue estos vínculos en el explorador del usuario de prueba para comprobar que el usuario de prueba no puede realizar estas operaciones.</span><span class="sxs-lookup"><span data-stu-id="a97c5-254">Paste these links into the test user's browser to verify the test user cannot perform these operations.</span></span>

## <a name="create-the-starter-app"></a><span data-ttu-id="a97c5-255">Crear la aplicación de inicio</span><span class="sxs-lookup"><span data-stu-id="a97c5-255">Create the starter app</span></span>

<span data-ttu-id="a97c5-256">Siga estas instrucciones para crear la aplicación de inicio.</span><span class="sxs-lookup"><span data-stu-id="a97c5-256">Follow these instructions to create the starter app.</span></span>

* <span data-ttu-id="a97c5-257">Crear un **aplicación Web de ASP.NET Core** con [2017 de Visual Studio](https://www.visualstudio.com/) denominado "ContactManager"</span><span class="sxs-lookup"><span data-stu-id="a97c5-257">Create an **ASP.NET Core Web Application** using [Visual Studio 2017](https://www.visualstudio.com/) named "ContactManager"</span></span>

  * <span data-ttu-id="a97c5-258">Crear la aplicación con **cuentas de usuario individuales**.</span><span class="sxs-lookup"><span data-stu-id="a97c5-258">Create the app with **Individual User Accounts**.</span></span>
  * <span data-ttu-id="a97c5-259">Asígnele el nombre "ContactManager" para que el espacio de nombres coincidirá con el uso de espacio de nombres en el ejemplo.</span><span class="sxs-lookup"><span data-stu-id="a97c5-259">Name it "ContactManager" so your namespace will match the namespace use in the sample.</span></span>

* <span data-ttu-id="a97c5-260">Agregue las siguientes `Contact` modelo:</span><span class="sxs-lookup"><span data-stu-id="a97c5-260">Add the following `Contact` model:</span></span>

  [!code-csharp[Main](secure-data/samples/starter/Models/Contact.cs?name=snippet1)]

* <span data-ttu-id="a97c5-261">Scaffold el `Contact` modelados con Entity Framework Core y `ApplicationDbContext` contexto de datos.</span><span class="sxs-lookup"><span data-stu-id="a97c5-261">Scaffold the `Contact` model using Entity Framework Core and the `ApplicationDbContext` data context.</span></span> <span data-ttu-id="a97c5-262">Acepte todos los valores predeterminados de la técnica scaffolding.</span><span class="sxs-lookup"><span data-stu-id="a97c5-262">Accept all the scaffolding defaults.</span></span> <span data-ttu-id="a97c5-263">Usar `ApplicationDbContext` para el contexto de datos de clase pone la tabla contacto la [identidad](xref:security/authentication/identity) base de datos.</span><span class="sxs-lookup"><span data-stu-id="a97c5-263">Using `ApplicationDbContext` for the data context class  puts the contact table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="a97c5-264">Vea [agregar un modelo](xref:tutorials/first-mvc-app/adding-model) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="a97c5-264">See [Adding a model](xref:tutorials/first-mvc-app/adding-model) for more information.</span></span>

* <span data-ttu-id="a97c5-265">Actualización del **ContactManager** fijar en el *Views/Shared/_Layout.cshtml* de archivos de `asp-controller="Home"` a `asp-controller="Contacts"` por lo que al puntear en el **ContactManager** vínculo invoca el controlador de contactos.</span><span class="sxs-lookup"><span data-stu-id="a97c5-265">Update the **ContactManager** anchor in the *Views/Shared/_Layout.cshtml* file from `asp-controller="Home"` to `asp-controller="Contacts"` so tapping the **ContactManager** link will invoke the Contacts controller.</span></span> <span data-ttu-id="a97c5-266">El marcado original:</span><span class="sxs-lookup"><span data-stu-id="a97c5-266">The original markup:</span></span>

```html
   <a asp-area="" asp-controller="Home" asp-action="Index" class="navbar-brand">ContactManager</a>
   ```

<span data-ttu-id="a97c5-267">El marcado actualizado:</span><span class="sxs-lookup"><span data-stu-id="a97c5-267">The updated markup:</span></span>

```html
   <a asp-area="" asp-controller="Contacts" asp-action="Index" class="navbar-brand">ContactManager</a>
   ```

* <span data-ttu-id="a97c5-268">Aplicar la técnica scaffolding la migración inicial y actualizar la base de datos</span><span class="sxs-lookup"><span data-stu-id="a97c5-268">Scaffold the initial migration and update the database</span></span>

```none
   dotnet ef migrations add initial
   dotnet ef database update
   ```

* <span data-ttu-id="a97c5-269">Probar la aplicación mediante la creación, edición y eliminación de un contacto</span><span class="sxs-lookup"><span data-stu-id="a97c5-269">Test the app by creating, editing and deleting a contact</span></span>

### <a name="seed-the-database"></a><span data-ttu-id="a97c5-270">Inicializar la base de datos</span><span class="sxs-lookup"><span data-stu-id="a97c5-270">Seed the database</span></span>

<span data-ttu-id="a97c5-271">Agregar el `SeedData` clase a la *datos* carpeta.</span><span class="sxs-lookup"><span data-stu-id="a97c5-271">Add the `SeedData` class to the *Data* folder.</span></span> <span data-ttu-id="a97c5-272">Si ha descargado el ejemplo, puede copiar la *SeedData.cs* del archivo a la *datos* carpeta del proyecto de inicio.</span><span class="sxs-lookup"><span data-stu-id="a97c5-272">If you've downloaded the sample, you can copy the *SeedData.cs* file to the *Data* folder of the starter project.</span></span>

[!code-csharp[Main](secure-data/samples/starter/Data/SeedData.cs)]

<span data-ttu-id="a97c5-273">Agregue el código resaltado hasta el final de la `Configure` método en el *Startup.cs* archivo:</span><span class="sxs-lookup"><span data-stu-id="a97c5-273">Add the highlighted code to the end of the `Configure` method in the *Startup.cs* file:</span></span>

[!code-csharp[Main](secure-data/samples/starter/Startup.cs?name=Configure&highlight=28-)]

<span data-ttu-id="a97c5-274">Compruebe que la aplicación había propagado la base de datos.</span><span class="sxs-lookup"><span data-stu-id="a97c5-274">Test that the app seeded the database.</span></span> <span data-ttu-id="a97c5-275">El método de inicialización no se ejecuta si no hay ninguna fila en la base de datos de contacto.</span><span class="sxs-lookup"><span data-stu-id="a97c5-275">The seed method doesn't run if there are any rows in the contact DB.</span></span>

### <a name="create-a-class-used-in-the-tutorial"></a><span data-ttu-id="a97c5-276">Crear una clase usada en el tutorial</span><span class="sxs-lookup"><span data-stu-id="a97c5-276">Create a class used in the tutorial</span></span>

* <span data-ttu-id="a97c5-277">Cree una carpeta denominada *autorización*.</span><span class="sxs-lookup"><span data-stu-id="a97c5-277">Create a folder named *Authorization*.</span></span>
* <span data-ttu-id="a97c5-278">Copia la *Authorization\ContactOperations.cs* de archivos desde el proyecto completado para descargar o copie el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="a97c5-278">Copy the *Authorization\ContactOperations.cs* file from the completed project download, or copy the following code:</span></span>

[!code-csharp[Main](secure-data/samples/final/Authorization/ContactOperations.cs)]

<a name="secure-data-add-resources-label"></a>

### <a name="additional-resources"></a><span data-ttu-id="a97c5-279">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="a97c5-279">Additional resources</span></span>

* <span data-ttu-id="a97c5-280">[Laboratorio de autorización de ASP.NET Core](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span><span class="sxs-lookup"><span data-stu-id="a97c5-280">[ASP.NET Core Authorization Lab](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span></span> <span data-ttu-id="a97c5-281">Este laboratorio se explica con más detalle en las características de seguridad introducidas en este tutorial.</span><span class="sxs-lookup"><span data-stu-id="a97c5-281">This lab goes into more detail on the security features introduced in this tutorial.</span></span>
* [<span data-ttu-id="a97c5-282">Autorización de ASP.NET Core: Simple, rol, basada en notificaciones y personalizado</span><span class="sxs-lookup"><span data-stu-id="a97c5-282">Authorization in ASP.NET Core : Simple, role, claims-based and custom</span></span>](index.md)
* [<span data-ttu-id="a97c5-283">Autorización personalizada basada en directivas</span><span class="sxs-lookup"><span data-stu-id="a97c5-283">Custom policy-based authorization</span></span>](policies.md)
