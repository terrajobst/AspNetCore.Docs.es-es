---
title: "Crear una aplicación de ASP.NET Core con datos de usuario protegidos por autorización"
author: rick-anderson
keywords: "Núcleo de ASP.NET, MVC, autorización, roles, seguridad, Administrador"
ms.author: riande
manager: wpickett
ms.date: 05/22/2017
ms.topic: article
ms.assetid: abeb2f8e-dfbf-4398-a04c-338a613a65bc
ms.technology: aspnet
ms.prod: aspnet-core
uid: security/authorization/secure-data
ms.openlocfilehash: 7ce59816d106cce9fabd8393ee764454bd0f7f27
ms.sourcegitcommit: 4e84d8bf5f404bb77f3d41665cf7e7374fc39142
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/05/2017
---
# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a><span data-ttu-id="671bc-103">Crear una aplicación de ASP.NET Core con datos de usuario protegidos por autorización</span><span class="sxs-lookup"><span data-stu-id="671bc-103">Create an ASP.NET Core app with user data protected by authorization</span></span>

<span data-ttu-id="671bc-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT) y [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="671bc-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="671bc-105">Este tutorial muestra cómo crear una aplicación web con datos de usuario protegidos mediante autorización.</span><span class="sxs-lookup"><span data-stu-id="671bc-105">This tutorial shows how to create a web app with user data protected by authorization.</span></span> <span data-ttu-id="671bc-106">Muestra una lista de contactos que autentica los usuarios (registrados) ha creado.</span><span class="sxs-lookup"><span data-stu-id="671bc-106">It  displays a list of contacts that authenticated (registered) users have created.</span></span> <span data-ttu-id="671bc-107">Hay tres grupos de seguridad:</span><span class="sxs-lookup"><span data-stu-id="671bc-107">There are three security groups:</span></span>

* <span data-ttu-id="671bc-108">Los usuarios registrados pueden ver todos los datos de contacto aprobados.</span><span class="sxs-lookup"><span data-stu-id="671bc-108">Registered users can view all the approved contact data.</span></span>
* <span data-ttu-id="671bc-109">Los usuarios registrados pueden editar o eliminar sus propios datos.</span><span class="sxs-lookup"><span data-stu-id="671bc-109">Registered users can edit/delete their own data.</span></span> 
* <span data-ttu-id="671bc-110">Los administradores pueden aprobar o rechazar datos de contacto.</span><span class="sxs-lookup"><span data-stu-id="671bc-110">Managers can approve or reject contact data.</span></span> <span data-ttu-id="671bc-111">Sólo los contactos aprobados son visibles para los usuarios.</span><span class="sxs-lookup"><span data-stu-id="671bc-111">Only approved contacts are visible to users.</span></span>
* <span data-ttu-id="671bc-112">Los administradores pueden aprobar o rechazar y editar o eliminar los datos.</span><span class="sxs-lookup"><span data-stu-id="671bc-112">Administrators can approve/reject and edit/delete any data.</span></span>

<span data-ttu-id="671bc-113">En la siguiente imagen, usuario Rick (`rick@example.com`) ha iniciado sesión.</span><span class="sxs-lookup"><span data-stu-id="671bc-113">In the following image, user Rick (`rick@example.com`) is signed in.</span></span> <span data-ttu-id="671bc-114">Usuario Rick solo pueden ver los contactos aprobados y editar o eliminar sus contactos.</span><span class="sxs-lookup"><span data-stu-id="671bc-114">User Rick can only view approved contacts and edit/delete his contacts.</span></span> <span data-ttu-id="671bc-115">El último registro, creado por Rick, muestra Editar y eliminar los vínculos</span><span class="sxs-lookup"><span data-stu-id="671bc-115">Only the last record, created by Rick, displays edit and delete links</span></span>

![imagen que se ha descrito anteriormente](secure-data/_static/rick.png)

<span data-ttu-id="671bc-117">En la siguiente imagen, `manager@contoso.com` está firmado en y en el rol de administrador.</span><span class="sxs-lookup"><span data-stu-id="671bc-117">In the following image, `manager@contoso.com` is signed in and in the managers role.</span></span> 

![imagen que se ha descrito anteriormente](secure-data/_static/manager1.png)

<span data-ttu-id="671bc-119">La siguiente imagen muestra a los administradores de la vista de detalles de un contacto.</span><span class="sxs-lookup"><span data-stu-id="671bc-119">The following image shows the  managers details view of a contact.</span></span>

![imagen que se ha descrito anteriormente](secure-data/_static/manager.png)

<span data-ttu-id="671bc-121">Sólo los administradores y los administradores tienen la aprobar y rechazan botones.</span><span class="sxs-lookup"><span data-stu-id="671bc-121">Only managers and administrators have the approve and reject buttons.</span></span>

<span data-ttu-id="671bc-122">En la siguiente imagen, `admin@contoso.com` está firmado en y en el rol del administrador.</span><span class="sxs-lookup"><span data-stu-id="671bc-122">In the following image, `admin@contoso.com` is signed in and in the administrator’s role.</span></span> 

![imagen que se ha descrito anteriormente](secure-data/_static/admin.png)

<span data-ttu-id="671bc-124">El administrador tiene todos los privilegios.</span><span class="sxs-lookup"><span data-stu-id="671bc-124">The administrator has all privileges.</span></span> <span data-ttu-id="671bc-125">Puede leer, editar o eliminar cualquier contacto y cambiar el estado de contactos.</span><span class="sxs-lookup"><span data-stu-id="671bc-125">She can read/edit/delete any contact and change the status of contacts.</span></span>

<span data-ttu-id="671bc-126">La aplicación se creó con [scaffolding](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller) siguiente `Contact` modelo:</span><span class="sxs-lookup"><span data-stu-id="671bc-126">The app was created by [scaffolding](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller)  the following `Contact` model:</span></span>

<span data-ttu-id="671bc-127">[!code-csharp[Main](secure-data/samples/starter/Models/Contact.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="671bc-127">[!code-csharp[Main](secure-data/samples/starter/Models/Contact.cs?name=snippet1)]</span></span>

<span data-ttu-id="671bc-128">Un `ContactIsOwnerAuthorizationHandler` controlador autorización garantiza que un usuario solo puede editar sus datos.</span><span class="sxs-lookup"><span data-stu-id="671bc-128">A `ContactIsOwnerAuthorizationHandler` authorization handler ensures that a user can only edit their data.</span></span> <span data-ttu-id="671bc-129">Un `ContactManagerAuthorizationHandler` controlador de autorización permite a los administradores aprobar o rechazar los contactos.</span><span class="sxs-lookup"><span data-stu-id="671bc-129">A `ContactManagerAuthorizationHandler` authorization handler allows managers to approve or reject contacts.</span></span>  <span data-ttu-id="671bc-130">Un `ContactAdministratorsAuthorizationHandler` controlador de autorización permite a los administradores para aprobar o rechazar los contactos y editar o eliminar contactos.</span><span class="sxs-lookup"><span data-stu-id="671bc-130">A `ContactAdministratorsAuthorizationHandler` authorization handler allows administrators to approve or reject contacts and to edit/delete contacts.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="671bc-131">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="671bc-131">Prerequisites</span></span>

<span data-ttu-id="671bc-132">Esto no es un tutorial de inicio.</span><span class="sxs-lookup"><span data-stu-id="671bc-132">This is not a beginning tutorial.</span></span> <span data-ttu-id="671bc-133">Debe estar familiarizado con:</span><span class="sxs-lookup"><span data-stu-id="671bc-133">You should be familiar with:</span></span>

* [<span data-ttu-id="671bc-134">Núcleo de ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="671bc-134">ASP.NET Core MVC</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="671bc-135">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="671bc-135">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

## <a name="the-starter-and-completed-app"></a><span data-ttu-id="671bc-136">El inicio y la aplicación completada</span><span class="sxs-lookup"><span data-stu-id="671bc-136">The starter and completed app</span></span>

<span data-ttu-id="671bc-137">[Descargar](xref:tutorials/index#how-to-download-a-sample) el [completado](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final) aplicación.</span><span class="sxs-lookup"><span data-stu-id="671bc-137">[Download](xref:tutorials/index#how-to-download-a-sample) the [completed](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final) app.</span></span> <span data-ttu-id="671bc-138">[Prueba](#test-the-completed-app) la aplicación completada por lo que se familiarice con sus características de seguridad.</span><span class="sxs-lookup"><span data-stu-id="671bc-138">[Test](#test-the-completed-app) the completed app so you become familiar with its security features.</span></span> 

### <a name="the-starter-app"></a><span data-ttu-id="671bc-139">La aplicación de inicio</span><span class="sxs-lookup"><span data-stu-id="671bc-139">The starter app</span></span>

<span data-ttu-id="671bc-140">Es útil comparar el código con el ejemplo completo.</span><span class="sxs-lookup"><span data-stu-id="671bc-140">It's helpful to compare your code with the completed sample.</span></span>

<span data-ttu-id="671bc-141">[Descargar](xref:tutorials/index#how-to-download-a-sample) el [starter](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter) aplicación.</span><span class="sxs-lookup"><span data-stu-id="671bc-141">[Download](xref:tutorials/index#how-to-download-a-sample) the [starter](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter) app.</span></span> 

<span data-ttu-id="671bc-142">Vea [crear la aplicación de inicio](#create-the-starter-app) si le gustaría crear desde cero.</span><span class="sxs-lookup"><span data-stu-id="671bc-142">See [Create the starter app](#create-the-starter-app) if you'd like to create it from scratch.</span></span>

<span data-ttu-id="671bc-143">Actualizar la base de datos:</span><span class="sxs-lookup"><span data-stu-id="671bc-143">Update the database:</span></span>

```none
   dotnet ef database update
```

<span data-ttu-id="671bc-144">Ejecutar la aplicación, pulse el **ContactManager** vincular y compruebe que puede crear, editar y eliminar un contacto.</span><span class="sxs-lookup"><span data-stu-id="671bc-144">Run the app, tap the **ContactManager** link, and verify you can create, edit, and delete a contact.</span></span>

<span data-ttu-id="671bc-145">Este tutorial tiene todos los pasos principales para crear la aplicación de datos de usuario seguras.</span><span class="sxs-lookup"><span data-stu-id="671bc-145">This tutorial has all the major steps to create the secure user data app.</span></span> <span data-ttu-id="671bc-146">Le resultará útil para hacer referencia al proyecto completado.</span><span class="sxs-lookup"><span data-stu-id="671bc-146">You may find it helpful to refer to the completed project.</span></span>

## <a name="modify-the-app-to-secure-user-data"></a><span data-ttu-id="671bc-147">Modificar la aplicación para proteger los datos de usuario</span><span class="sxs-lookup"><span data-stu-id="671bc-147">Modify the app to secure user data</span></span>

<span data-ttu-id="671bc-148">Las siguientes secciones contienen todos los pasos principales para crear la aplicación de datos de usuario seguras.</span><span class="sxs-lookup"><span data-stu-id="671bc-148">The following sections have all the major steps to create the secure user data app.</span></span> <span data-ttu-id="671bc-149">Le resultará útil para hacer referencia al proyecto completado.</span><span class="sxs-lookup"><span data-stu-id="671bc-149">You may find it helpful to refer to the completed project.</span></span>

### <a name="tie-the-contact-data-to-the-user"></a><span data-ttu-id="671bc-150">Asociar los datos de contacto para el usuario</span><span class="sxs-lookup"><span data-stu-id="671bc-150">Tie the contact data to the user</span></span>

<span data-ttu-id="671bc-151">Usar ASP.NET [identidad](xref:security/authentication/identity) Id. de usuario para garantizar que los usuarios puede editar sus datos, pero no otros datos de los usuarios.</span><span class="sxs-lookup"><span data-stu-id="671bc-151">Use the ASP.NET [Identity](xref:security/authentication/identity) user ID to ensure users can edit their data, but not other users data.</span></span> <span data-ttu-id="671bc-152">Agregar `OwnerID` a la `Contact` modelo:</span><span class="sxs-lookup"><span data-stu-id="671bc-152">Add `OwnerID` to the `Contact` model:</span></span>

<span data-ttu-id="671bc-153">[!code-csharp[Main](secure-data/samples/final/Models/Contact.cs?name=snippet1&highlight=5-6,16-)]</span><span class="sxs-lookup"><span data-stu-id="671bc-153">[!code-csharp[Main](secure-data/samples/final/Models/Contact.cs?name=snippet1&highlight=5-6,16-)]</span></span>

<span data-ttu-id="671bc-154">`OwnerID`es el identificador del usuario desde el `AspNetUser` tabla el [identidad](xref:security/authentication/identity) base de datos.</span><span class="sxs-lookup"><span data-stu-id="671bc-154">`OwnerID` is the user's ID from the `AspNetUser` table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="671bc-155">El `Status` campo determina si un contacto es visible para los usuarios en general.</span><span class="sxs-lookup"><span data-stu-id="671bc-155">The `Status` field determines if a contact is viewable by general users.</span></span> 

<span data-ttu-id="671bc-156">Aplicar la técnica scaffolding una nueva migración y actualizar la base de datos:</span><span class="sxs-lookup"><span data-stu-id="671bc-156">Scaffold a new migration and update the database:</span></span>

```console
dotnet ef migrations add userID_Status
dotnet ef database update
 ```

### <a name="require-ssl-and-authenticated-users"></a><span data-ttu-id="671bc-157">Requerir SSL y los usuarios autenticados</span><span class="sxs-lookup"><span data-stu-id="671bc-157">Require SSL and authenticated users</span></span>

<span data-ttu-id="671bc-158">En el `ConfigureServices` método de la *Startup.cs* , agregue el [RequireHttpsAttribute](http://docs.asp.net/projects/api/en/latest/autoapi/Microsoft/AspNetCore/Mvc/RequireHttpsAttribute/index.html.md#Microsoft.AspNetCore.Mvc.RequireHttpsAttribute.md) filtro de autorización:</span><span class="sxs-lookup"><span data-stu-id="671bc-158">In the `ConfigureServices` method of the *Startup.cs* file, add the [RequireHttpsAttribute](http://docs.asp.net/projects/api/en/latest/autoapi/Microsoft/AspNetCore/Mvc/RequireHttpsAttribute/index.html.md#Microsoft.AspNetCore.Mvc.RequireHttpsAttribute.md) authorization filter:</span></span>

<span data-ttu-id="671bc-159">[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=snippet_SSL&highlight=1)]</span><span class="sxs-lookup"><span data-stu-id="671bc-159">[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=snippet_SSL&highlight=1)]</span></span>

<span data-ttu-id="671bc-160">Si está utilizando Visual Studio, consulte [configurar IIS Express para HTTPS/SSL](xref:security/enforcing-ssl#set-up-iis-express-for-sslhttps).</span><span class="sxs-lookup"><span data-stu-id="671bc-160">If you're using Visual Studio, see [Set up IIS Express for SSL/HTTPS](xref:security/enforcing-ssl#set-up-iis-express-for-sslhttps).</span></span> <span data-ttu-id="671bc-161">Para redirigir las solicitudes HTTP a HTTPS, consulte [Middleware de reescritura de dirección URL](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="671bc-161">To redirect HTTP requests to HTTPS, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span> <span data-ttu-id="671bc-162">Si está utilizando Visual Studio Code o pruebas en plataforma local no incluye un certificado de prueba para SSL:</span><span class="sxs-lookup"><span data-stu-id="671bc-162">If you are using Visual Studio Code or testing on local platform that doesn't include a test certificate for SSL:</span></span>

- <span data-ttu-id="671bc-163">Establecer `"LocalTest:skipSSL": true` en el *appSettings.JSON que se* archivo.</span><span class="sxs-lookup"><span data-stu-id="671bc-163">Set `"LocalTest:skipSSL": true` in the *appsettings.json* file.</span></span>

### <a name="require-authenticated-users"></a><span data-ttu-id="671bc-164">Requerir a los usuarios autenticados</span><span class="sxs-lookup"><span data-stu-id="671bc-164">Require authenticated users</span></span>

<span data-ttu-id="671bc-165">Establecer la directiva de autenticación predeterminado para exigir que los usuarios se autentiquen.</span><span class="sxs-lookup"><span data-stu-id="671bc-165">Set the default authentication policy to require users to be authenticated.</span></span> <span data-ttu-id="671bc-166">Puede rechazar la autenticación en el método de acción o controlador con el `[AllowAnonymous]` atributo.</span><span class="sxs-lookup"><span data-stu-id="671bc-166">You can opt out of authentication at the controller or action method with the `[AllowAnonymous]` attribute.</span></span> <span data-ttu-id="671bc-167">Con este enfoque, los nuevos controladores que se agrega automáticamente requerirá la autenticación, que es más segura que confiar en los nuevos controladores para incluir la `[Authorize]` atributo.</span><span class="sxs-lookup"><span data-stu-id="671bc-167">With this approach, any new controllers added will automatically require authentication, which is safer than relying on new controllers to include the `[Authorize]` attribute.</span></span> <span data-ttu-id="671bc-168">Agregue lo siguiente a la `ConfigureServices` método de la *Startup.cs* archivo:</span><span class="sxs-lookup"><span data-stu-id="671bc-168">Add the following to  the `ConfigureServices` method of the *Startup.cs* file:</span></span>

<span data-ttu-id="671bc-169">[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=snippet_defaultPolicy)]</span><span class="sxs-lookup"><span data-stu-id="671bc-169">[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=snippet_defaultPolicy)]</span></span>

<span data-ttu-id="671bc-170">Agregar `[AllowAnonymous]` al controlador home para los usuarios anónimos pueden recibir información sobre el sitio antes de que registrar.</span><span class="sxs-lookup"><span data-stu-id="671bc-170">Add `[AllowAnonymous]` to the home controller so anonymous users can get information about the site before they register.</span></span>

<span data-ttu-id="671bc-171">[!code-csharp[Main](secure-data/samples/final/Controllers/HomeController.cs?name=snippet1&highlight=2,6)]</span><span class="sxs-lookup"><span data-stu-id="671bc-171">[!code-csharp[Main](secure-data/samples/final/Controllers/HomeController.cs?name=snippet1&highlight=2,6)]</span></span>

### <a name="configure-the-test-account"></a><span data-ttu-id="671bc-172">Configurar la cuenta de prueba</span><span class="sxs-lookup"><span data-stu-id="671bc-172">Configure the test account</span></span>

<span data-ttu-id="671bc-173">La `SeedData` clase crea dos cuentas, el administrador y el administrador.</span><span class="sxs-lookup"><span data-stu-id="671bc-173">The `SeedData` class creates two accounts,  administrator and manager.</span></span> <span data-ttu-id="671bc-174">Use la [herramienta secreto administrador](xref:security/app-secrets) para establecer una contraseña para estas cuentas.</span><span class="sxs-lookup"><span data-stu-id="671bc-174">Use the [Secret Manager tool](xref:security/app-secrets) to set a password for these accounts.</span></span> <span data-ttu-id="671bc-175">Desde el directorio del proyecto (el directorio que contiene *Program.cs*).</span><span class="sxs-lookup"><span data-stu-id="671bc-175">Do this from the project directory (the directory containing *Program.cs*).</span></span>

```console
dotnet user-secrets set SeedUserPW <PW>
```

<span data-ttu-id="671bc-176">Actualización `Configure` usar la contraseña de prueba:</span><span class="sxs-lookup"><span data-stu-id="671bc-176">Update `Configure` to use the test password:</span></span>

<span data-ttu-id="671bc-177">[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=Configure&highlight=19-21)]</span><span class="sxs-lookup"><span data-stu-id="671bc-177">[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=Configure&highlight=19-21)]</span></span>

<span data-ttu-id="671bc-178">Agregue el identificador de usuario de administrador y `Status = ContactStatus.Approved` a los contactos.</span><span class="sxs-lookup"><span data-stu-id="671bc-178">Add the administrator user ID and `Status = ContactStatus.Approved` to the contacts.</span></span> <span data-ttu-id="671bc-179">Se muestra sólo un contacto, agregue el identificador de usuario para todos los contactos:</span><span class="sxs-lookup"><span data-stu-id="671bc-179">Only one contact is shown, add the user ID to all contacts:</span></span>

<span data-ttu-id="671bc-180">[!code-csharp[Main](secure-data/samples/final/Data/SeedData.cs?name=snippet1&highlight=17,18)]</span><span class="sxs-lookup"><span data-stu-id="671bc-180">[!code-csharp[Main](secure-data/samples/final/Data/SeedData.cs?name=snippet1&highlight=17,18)]</span></span>

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a><span data-ttu-id="671bc-181">Crear propietario, el administrador y los controladores de autorización de administrador</span><span class="sxs-lookup"><span data-stu-id="671bc-181">Create owner, manager, and administrator authorization handlers</span></span>

<span data-ttu-id="671bc-182">Crear un `ContactIsOwnerAuthorizationHandler` clase en el *autorización* carpeta.</span><span class="sxs-lookup"><span data-stu-id="671bc-182">Create a `ContactIsOwnerAuthorizationHandler` class in the  *Authorization* folder.</span></span> <span data-ttu-id="671bc-183">El `ContactIsOwnerAuthorizationHandler` comprobará que el usuario que actúa en el recurso es propietario del recurso.</span><span class="sxs-lookup"><span data-stu-id="671bc-183">The `ContactIsOwnerAuthorizationHandler` will verify the user acting on the resource owns the resource.</span></span>

<span data-ttu-id="671bc-184">[!code-csharp[Main](secure-data/samples/final/Authorization/ContactIsOwnerAuthorizationHandler.cs)]</span><span class="sxs-lookup"><span data-stu-id="671bc-184">[!code-csharp[Main](secure-data/samples/final/Authorization/ContactIsOwnerAuthorizationHandler.cs)]</span></span>

<span data-ttu-id="671bc-185">El `ContactIsOwnerAuthorizationHandler` llamadas `context.Succeed` si el usuario autenticado actual es el propietario del contacto.</span><span class="sxs-lookup"><span data-stu-id="671bc-185">The `ContactIsOwnerAuthorizationHandler` calls `context.Succeed` if the current authenticated user is the contact owner.</span></span> <span data-ttu-id="671bc-186">Controladores de autorización generalmente devuelven `context.Succeed` cuando se cumplen los requisitos.</span><span class="sxs-lookup"><span data-stu-id="671bc-186">Authorization handlers generally return `context.Succeed` when the requirements are met.</span></span> <span data-ttu-id="671bc-187">Devuelven `Task.FromResult(0)` cuando no se cumplen los requisitos.</span><span class="sxs-lookup"><span data-stu-id="671bc-187">They return `Task.FromResult(0)` when requirements are not met.</span></span> <span data-ttu-id="671bc-188">`Task.FromResult(0)`no correcto o error, permite que otro controlador de autorización para que se ejecute.</span><span class="sxs-lookup"><span data-stu-id="671bc-188">`Task.FromResult(0)` is neither success or failure, it allows other authorization handler to run.</span></span> <span data-ttu-id="671bc-189">Si necesita explícitamente un error, devuelve `context.Fail()`.</span><span class="sxs-lookup"><span data-stu-id="671bc-189">If you need to explicitly fail, return `context.Fail()`.</span></span>

<span data-ttu-id="671bc-190">Se permiten los propietarios de contacto editar o eliminar sus propios datos, por lo que no es necesario comprobar la operación pasada en el parámetro de requisito.</span><span class="sxs-lookup"><span data-stu-id="671bc-190">We allow contact owners to edit/delete their own data, so we don't need to check the operation passed in the requirement parameter.</span></span>

### <a name="create-a-manager-authorization-handler"></a><span data-ttu-id="671bc-191">Crear un controlador del Administrador de autorización</span><span class="sxs-lookup"><span data-stu-id="671bc-191">Create a manager authorization handler</span></span>

<span data-ttu-id="671bc-192">Crear un `ContactManagerAuthorizationHandler` clase en el *autorización* carpeta.</span><span class="sxs-lookup"><span data-stu-id="671bc-192">Create a `ContactManagerAuthorizationHandler` class in the  *Authorization* folder.</span></span> <span data-ttu-id="671bc-193">El `ContactManagerAuthorizationHandler` comprobará que el usuario que actúa en el recurso es un administrador.</span><span class="sxs-lookup"><span data-stu-id="671bc-193">The `ContactManagerAuthorizationHandler` will verify the user acting on the resource is a manager.</span></span> <span data-ttu-id="671bc-194">Solo los administradores pueden aprobar o rechazar los cambios de contenido (nuevos o modificados).</span><span class="sxs-lookup"><span data-stu-id="671bc-194">Only managers can approve or reject content changes (new or changed).</span></span>

<span data-ttu-id="671bc-195">[!code-csharp[Main](secure-data/samples/final/Authorization/ContactManagerAuthorizationHandler.cs)]</span><span class="sxs-lookup"><span data-stu-id="671bc-195">[!code-csharp[Main](secure-data/samples/final/Authorization/ContactManagerAuthorizationHandler.cs)]</span></span>

### <a name="create-an-administrator-authorization-handler"></a><span data-ttu-id="671bc-196">Crear un controlador de autorización de administrador</span><span class="sxs-lookup"><span data-stu-id="671bc-196">Create an administrator authorization handler</span></span>

<span data-ttu-id="671bc-197">Crear un `ContactAdministratorsAuthorizationHandler` clase en el *autorización* carpeta.</span><span class="sxs-lookup"><span data-stu-id="671bc-197">Create a `ContactAdministratorsAuthorizationHandler` class in the  *Authorization* folder.</span></span> <span data-ttu-id="671bc-198">El `ContactAdministratorsAuthorizationHandler` comprobará que el usuario que actúa en el recurso es un administrador.</span><span class="sxs-lookup"><span data-stu-id="671bc-198">The `ContactAdministratorsAuthorizationHandler` will verify the user acting on the resource is a administrator.</span></span> <span data-ttu-id="671bc-199">Administrador puede hacer que todas las operaciones.</span><span class="sxs-lookup"><span data-stu-id="671bc-199">Administrator can do all operations.</span></span>

<span data-ttu-id="671bc-200">[!code-csharp[Main](secure-data/samples/final/Authorization/ContactAdministratorsAuthorizationHandler.cs)]</span><span class="sxs-lookup"><span data-stu-id="671bc-200">[!code-csharp[Main](secure-data/samples/final/Authorization/ContactAdministratorsAuthorizationHandler.cs)]</span></span>

## <a name="register-the-authorization-handlers"></a><span data-ttu-id="671bc-201">Registrar los controladores de autorización</span><span class="sxs-lookup"><span data-stu-id="671bc-201">Register the authorization handlers</span></span>

<span data-ttu-id="671bc-202">Servicios mediante Entity Framework Core deben estar registrados para [inyección de dependencia](xref:fundamentals/dependency-injection) con [AddScoped](http://docs.asp.net/projects/api/en/latest/autoapi/Microsoft/Extensions/DependencyInjection/ServiceCollectionServiceExtensions/index.html.md#Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddScoped.md).</span><span class="sxs-lookup"><span data-stu-id="671bc-202">Services using Entity Framework Core must be registered for [dependency injection](xref:fundamentals/dependency-injection) using [AddScoped](http://docs.asp.net/projects/api/en/latest/autoapi/Microsoft/Extensions/DependencyInjection/ServiceCollectionServiceExtensions/index.html.md#Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddScoped.md).</span></span> <span data-ttu-id="671bc-203">El `ContactIsOwnerAuthorizationHandler` usa ASP.NET Core [identidad](xref:security/authentication/identity), que se basa en Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="671bc-203">The `ContactIsOwnerAuthorizationHandler` uses ASP.NET Core [Identity](xref:security/authentication/identity), which is built on Entity Framework Core.</span></span> <span data-ttu-id="671bc-204">Registre los controladores con la colección de servicio para que estén disponibles para la `ContactsController` a través de [inyección de dependencia](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="671bc-204">Register the handlers with the service collection so they will be available to the `ContactsController` through [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="671bc-205">Agregue el código siguiente al final de `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="671bc-205">Add the following code to the end of `ConfigureServices`:</span></span>

<span data-ttu-id="671bc-206">[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=AuthorizationHandlers)]</span><span class="sxs-lookup"><span data-stu-id="671bc-206">[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=AuthorizationHandlers)]</span></span>

<span data-ttu-id="671bc-207">`ContactAdministratorsAuthorizationHandler`y `ContactManagerAuthorizationHandler` se agregan como singleton.</span><span class="sxs-lookup"><span data-stu-id="671bc-207">`ContactAdministratorsAuthorizationHandler` and `ContactManagerAuthorizationHandler` are added as singletons.</span></span> <span data-ttu-id="671bc-208">Son singletons porque no usan EF y toda la información necesaria se encuentra en la `Context` parámetro de la `HandleRequirementAsync` método.</span><span class="sxs-lookup"><span data-stu-id="671bc-208">They are singletons because they don't use EF and all the information needed is in the `Context` parameter of the `HandleRequirementAsync` method.</span></span>

<span data-ttu-id="671bc-209">La sección completa `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="671bc-209">The complete `ConfigureServices`:</span></span>

<span data-ttu-id="671bc-210">[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=ConfigureServices)]</span><span class="sxs-lookup"><span data-stu-id="671bc-210">[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=ConfigureServices)]</span></span>

## <a name="update-the-code-to-support-authorization"></a><span data-ttu-id="671bc-211">Actualizar el código para admitir la autorización</span><span class="sxs-lookup"><span data-stu-id="671bc-211">Update the code to support authorization</span></span>

<span data-ttu-id="671bc-212">En esta sección, actualice el controlador y vistas y agregar una clase de requisitos de las operaciones.</span><span class="sxs-lookup"><span data-stu-id="671bc-212">In this section, you update the controller and views and add an operations requirements class.</span></span>

### <a name="update-the-contacts-controller"></a><span data-ttu-id="671bc-213">Actualice el controlador de contactos</span><span class="sxs-lookup"><span data-stu-id="671bc-213">Update the Contacts controller</span></span>

<span data-ttu-id="671bc-214">Actualización de la `ContactsController` constructor:</span><span class="sxs-lookup"><span data-stu-id="671bc-214">Update the `ContactsController` constructor:</span></span>

* <span data-ttu-id="671bc-215">Agregar el `IAuthorizationService` servicio para tener acceso a los controladores de autorización.</span><span class="sxs-lookup"><span data-stu-id="671bc-215">Add the `IAuthorizationService` service to  access to the authorization handlers.</span></span> 
* <span data-ttu-id="671bc-216">Agregar el `Identity` `UserManager` servicio:</span><span class="sxs-lookup"><span data-stu-id="671bc-216">Add the `Identity` `UserManager` service:</span></span>

<span data-ttu-id="671bc-217">[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_ContactsControllerCtor)]</span><span class="sxs-lookup"><span data-stu-id="671bc-217">[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_ContactsControllerCtor)]</span></span>

### <a name="add-a-contact-operations-requirements-class"></a><span data-ttu-id="671bc-218">Agregar una clase de requisitos de operaciones de contacto</span><span class="sxs-lookup"><span data-stu-id="671bc-218">Add a contact operations requirements class</span></span>

<span data-ttu-id="671bc-219">Agregar el `ContactOperationsRequirements` clase a la *autorización* carpeta.</span><span class="sxs-lookup"><span data-stu-id="671bc-219">Add the `ContactOperationsRequirements` class to the *Authorization* folder.</span></span> <span data-ttu-id="671bc-220">Esta clase contiene los requisitos de la aplicación admite:</span><span class="sxs-lookup"><span data-stu-id="671bc-220">This class  contain the requirements our app supports:</span></span>

<span data-ttu-id="671bc-221">[!code-csharp[Main](secure-data/samples/final/Authorization/ContactOperations.cs)]</span><span class="sxs-lookup"><span data-stu-id="671bc-221">[!code-csharp[Main](secure-data/samples/final/Authorization/ContactOperations.cs)]</span></span>

### <a name="update-create"></a><span data-ttu-id="671bc-222">Crear actualización</span><span class="sxs-lookup"><span data-stu-id="671bc-222">Update Create</span></span>

<span data-ttu-id="671bc-223">Actualización de la `HTTP POST Create` método:</span><span class="sxs-lookup"><span data-stu-id="671bc-223">Update the `HTTP POST Create` method to:</span></span>

* <span data-ttu-id="671bc-224">Agregar el identificador de usuario para el `Contact` modelo.</span><span class="sxs-lookup"><span data-stu-id="671bc-224">Add the user ID to the `Contact` model.</span></span>
* <span data-ttu-id="671bc-225">Llamar al controlador de autorización para comprobar que el usuario es propietario del contacto.</span><span class="sxs-lookup"><span data-stu-id="671bc-225">Call the authorization handler to verify the user owns the contact.</span></span>

<span data-ttu-id="671bc-226">[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_Create)]</span><span class="sxs-lookup"><span data-stu-id="671bc-226">[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_Create)]</span></span>

### <a name="update-edit"></a><span data-ttu-id="671bc-227">Edición de actualización</span><span class="sxs-lookup"><span data-stu-id="671bc-227">Update Edit</span></span>

<span data-ttu-id="671bc-228">Actualizarlas `Edit` métodos para usar el controlador de autorización para comprobar que el usuario propietario del contacto.</span><span class="sxs-lookup"><span data-stu-id="671bc-228">Update both `Edit` methods to use the authorization handler to verify the user owns the contact.</span></span> <span data-ttu-id="671bc-229">Dado que se está realizando la autorización de recurso no podemos usar la `[Authorize]` atributo.</span><span class="sxs-lookup"><span data-stu-id="671bc-229">Because we are performing resource authorization we cannot use the `[Authorize]` attribute.</span></span> <span data-ttu-id="671bc-230">No tenemos acceso a los recursos cuando se evalúan los atributos.</span><span class="sxs-lookup"><span data-stu-id="671bc-230">We don't have access to the resource when attributes are evaluated.</span></span> <span data-ttu-id="671bc-231">Autorización basada en recursos debe ser imperativo.</span><span class="sxs-lookup"><span data-stu-id="671bc-231">Resource based authorization must be imperative.</span></span> <span data-ttu-id="671bc-232">Las comprobaciones deben realizarse una vez que tenemos acceso a los recursos, cargándolo en un control o mediante la carga dentro de su propio controlador.</span><span class="sxs-lookup"><span data-stu-id="671bc-232">Checks must be performed once we have access to the resource, either by loading it in our controller, or by loading it within the handler itself.</span></span> <span data-ttu-id="671bc-233">Con frecuencia tendrán acceso al recurso pasando la clave de recurso.</span><span class="sxs-lookup"><span data-stu-id="671bc-233">Frequently you will access the resource by passing in the resource key.</span></span>

<span data-ttu-id="671bc-234">[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_Edit)]</span><span class="sxs-lookup"><span data-stu-id="671bc-234">[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_Edit)]</span></span>

### <a name="update-the-delete-method"></a><span data-ttu-id="671bc-235">El método Delete de actualización</span><span class="sxs-lookup"><span data-stu-id="671bc-235">Update the Delete method</span></span>

<span data-ttu-id="671bc-236">Actualizarlas `Delete` métodos para usar el controlador de autorización para comprobar que el usuario propietario del contacto.</span><span class="sxs-lookup"><span data-stu-id="671bc-236">Update both `Delete` methods to use the authorization handler to verify the user owns the contact.</span></span>

<span data-ttu-id="671bc-237">[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_Delete)]</span><span class="sxs-lookup"><span data-stu-id="671bc-237">[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_Delete)]</span></span>

## <a name="inject-the-authorization-service-into-the-views"></a><span data-ttu-id="671bc-238">Insertar el servicio de autorización en las vistas</span><span class="sxs-lookup"><span data-stu-id="671bc-238">Inject the authorization service into the views</span></span>

<span data-ttu-id="671bc-239">Actualmente se muestra en la interfaz de usuario edita y elimina los vínculos de datos que no se puede modificar el usuario.</span><span class="sxs-lookup"><span data-stu-id="671bc-239">Currently the UI shows edit and delete links for data the user cannot modify.</span></span> <span data-ttu-id="671bc-240">Se corregirá aplicando el controlador de autorización a las vistas.</span><span class="sxs-lookup"><span data-stu-id="671bc-240">We'll fix that by applying the authorization handler to the views.</span></span>

<span data-ttu-id="671bc-241">Insertar el servicio de autorización en el *Views/_ViewImports.cshtml* por lo que estará disponible para todas las vistas de archivos:</span><span class="sxs-lookup"><span data-stu-id="671bc-241">Inject the authorization service in the *Views/_ViewImports.cshtml* file so it will be available to all views:</span></span>

<span data-ttu-id="671bc-242">[!code-html[Main](secure-data/samples/final/Views/_ViewImports.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="671bc-242">[!code-html[Main](secure-data/samples/final/Views/_ViewImports.cshtml)]</span></span>

<span data-ttu-id="671bc-243">Actualización de la *Views/Contacts/Index.cshtml* vista Razor para mostrar la edición solo y eliminar los vínculos para los usuarios que pueden el contacto de editar o eliminar.</span><span class="sxs-lookup"><span data-stu-id="671bc-243">Update the *Views/Contacts/Index.cshtml* Razor view to only display the edit and delete links for users who can edit/delete the contact.</span></span>

<span data-ttu-id="671bc-244">Agregar`@using ContactManager.Authorization;`</span><span class="sxs-lookup"><span data-stu-id="671bc-244">Add `@using ContactManager.Authorization;`</span></span>

<span data-ttu-id="671bc-245">Actualización de la `Edit` y `Delete` vincula por lo que solo se representan para que los usuarios con permiso Modificar y eliminar el contacto.</span><span class="sxs-lookup"><span data-stu-id="671bc-245">Update the `Edit` and `Delete` links so they are only rendered for users with permission to edit and delete the contact.</span></span>

<span data-ttu-id="671bc-246">[!code-html[Main](secure-data/samples/final/Views/Contacts/Index.cshtml?range=63-84)]</span><span class="sxs-lookup"><span data-stu-id="671bc-246">[!code-html[Main](secure-data/samples/final/Views/Contacts/Index.cshtml?range=63-84)]</span></span>

<span data-ttu-id="671bc-247">Advertencia: Ocultar los vínculos de los usuarios que no tienen permiso para editar o eliminar los datos no protege la aplicación.</span><span class="sxs-lookup"><span data-stu-id="671bc-247">Warning: Hiding links from users that do not have permission to edit or delete data does not secure the app.</span></span> <span data-ttu-id="671bc-248">Ocultar los vínculos hace que la aplicación de usuario más descriptivo mostrando vínculos solo es válido.</span><span class="sxs-lookup"><span data-stu-id="671bc-248">Hiding links makes the app more user friendly by displaying only valid links.</span></span> <span data-ttu-id="671bc-249">Los usuarios pueden hack las direcciones URL generadas para invocar Editar y eliminar operaciones en los datos que no poseen.</span><span class="sxs-lookup"><span data-stu-id="671bc-249">Users can hack the generated URLs to invoke edit and delete operations on data they don't own.</span></span>  <span data-ttu-id="671bc-250">El controlador debe repetir que comprueba el acceso para que sea seguro.</span><span class="sxs-lookup"><span data-stu-id="671bc-250">The controller must repeat the access checks to be secure.</span></span>

### <a name="update-the-details-view"></a><span data-ttu-id="671bc-251">Actualizar la vista de detalles</span><span class="sxs-lookup"><span data-stu-id="671bc-251">Update the Details view</span></span>

<span data-ttu-id="671bc-252">Actualice la vista de detalles para que los administradores pueden aprobar o rechazar contactos:</span><span class="sxs-lookup"><span data-stu-id="671bc-252">Update the details view so managers can approve or reject contacts:</span></span>

<span data-ttu-id="671bc-253">[!code-html[Main](secure-data/samples/final/Views/Contacts/Details.cshtml?range=53-)]</span><span class="sxs-lookup"><span data-stu-id="671bc-253">[!code-html[Main](secure-data/samples/final/Views/Contacts/Details.cshtml?range=53-)]</span></span>

## <a name="test-the-completed-app"></a><span data-ttu-id="671bc-254">Probar la aplicación completada</span><span class="sxs-lookup"><span data-stu-id="671bc-254">Test the completed app</span></span>

<span data-ttu-id="671bc-255">Si está utilizando Visual Studio Code o pruebas en plataforma local no incluye un certificado de prueba para SSL:</span><span class="sxs-lookup"><span data-stu-id="671bc-255">If you are using Visual Studio Code or testing on local platform that doesn't include a test certificate for SSL:</span></span>

- <span data-ttu-id="671bc-256">Establecer `"LocalTest:skipSSL": true` en el *appSettings.JSON que se* archivo.</span><span class="sxs-lookup"><span data-stu-id="671bc-256">Set `"LocalTest:skipSSL": true` in the *appsettings.json* file.</span></span>

<span data-ttu-id="671bc-257">Si se ha ejecutado la aplicación y tener contactos, elimine todos los registros en el `Contact` de tabla y reinicie la aplicación para inicializar la base de datos.</span><span class="sxs-lookup"><span data-stu-id="671bc-257">If you have run the app and have contacts, delete all the records in the `Contact` table and restart the app to seed the database.</span></span> <span data-ttu-id="671bc-258">Si se utiliza Visual Studio, debe salir y reiniciar IIS Express al valor de inicialización de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="671bc-258">If you are using Visual Studio, you need to exit and restart IIS Express to seed the database.</span></span>

<span data-ttu-id="671bc-259">Registrar un usuario para examinar los contactos.</span><span class="sxs-lookup"><span data-stu-id="671bc-259">Register a user to browse the contacts.</span></span>

<span data-ttu-id="671bc-260">Una manera sencilla de probar la aplicación final consiste en iniciar tres distintos exploradores (o versiones de incógnito/InPrivate).</span><span class="sxs-lookup"><span data-stu-id="671bc-260">An easy way to test the completed app is to launch three different browsers (or incognito/InPrivate versions).</span></span> <span data-ttu-id="671bc-261">En un explorador, registrar un nuevo usuario, por ejemplo, `test@example.com`.</span><span class="sxs-lookup"><span data-stu-id="671bc-261">In one browser, register a new user, for example, `test@example.com`.</span></span> <span data-ttu-id="671bc-262">Inicie sesión con un usuario diferente en cada explorador.</span><span class="sxs-lookup"><span data-stu-id="671bc-262">Sign in to each browser with a different user.</span></span> <span data-ttu-id="671bc-263">Compruebe lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="671bc-263">Verify the following:</span></span>

* <span data-ttu-id="671bc-264">Los usuarios registrados pueden ver todos los datos de contacto aprobados.</span><span class="sxs-lookup"><span data-stu-id="671bc-264">Registered users can view all the approved contact data.</span></span>
* <span data-ttu-id="671bc-265">Los usuarios registrados pueden editar o eliminar sus propios datos.</span><span class="sxs-lookup"><span data-stu-id="671bc-265">Registered users can edit/delete their own data.</span></span> 
* <span data-ttu-id="671bc-266">Los administradores pueden aprobar o rechazar datos de contacto.</span><span class="sxs-lookup"><span data-stu-id="671bc-266">Managers can approve or reject contact data.</span></span> <span data-ttu-id="671bc-267">El `Details` vista muestra **aprobar** y **rechazar** botones.</span><span class="sxs-lookup"><span data-stu-id="671bc-267">The `Details` view shows **Approve** and **Reject** buttons.</span></span> 
* <span data-ttu-id="671bc-268">Los administradores pueden aprobar o rechazar y editar o eliminar los datos.</span><span class="sxs-lookup"><span data-stu-id="671bc-268">Administrators can approve/reject and edit/delete any data.</span></span>

| <span data-ttu-id="671bc-269">Usuario</span><span class="sxs-lookup"><span data-stu-id="671bc-269">User</span></span>| <span data-ttu-id="671bc-270">Opciones</span><span class="sxs-lookup"><span data-stu-id="671bc-270">Options</span></span> |
| ------------ | ---------|
| test@example.com | <span data-ttu-id="671bc-271">Editar puede/eliminar datos propios</span><span class="sxs-lookup"><span data-stu-id="671bc-271">Can edit/delete own data</span></span> |
| manager@contoso.com | <span data-ttu-id="671bc-272">Pueden aprobar o rechazar y editar o eliminar datos de propietario</span><span class="sxs-lookup"><span data-stu-id="671bc-272">Can approve/reject and edit/delete own data</span></span>  |
| admin@contoso.com | <span data-ttu-id="671bc-273">Puede editar o eliminar y aprobar o rechazar todos los datos</span><span class="sxs-lookup"><span data-stu-id="671bc-273">Can edit/delete and approve/reject all data</span></span>|

<span data-ttu-id="671bc-274">Crear un contacto en el Explorador de administradores.</span><span class="sxs-lookup"><span data-stu-id="671bc-274">Create a contact in the administrators browser.</span></span> <span data-ttu-id="671bc-275">Copie la dirección URL para eliminar y editar en el contacto del administrador.</span><span class="sxs-lookup"><span data-stu-id="671bc-275">Copy the URL for delete and edit from the administrator contact.</span></span> <span data-ttu-id="671bc-276">Pegue estos vínculos en el explorador del usuario de prueba para comprobar que el usuario de prueba no puede realizar estas operaciones.</span><span class="sxs-lookup"><span data-stu-id="671bc-276">Paste these links into the test user's browser to verify the test user cannot perform these operations.</span></span>

## <a name="create-the-starter-app"></a><span data-ttu-id="671bc-277">Crear la aplicación de inicio</span><span class="sxs-lookup"><span data-stu-id="671bc-277">Create the starter app</span></span>

<span data-ttu-id="671bc-278">Siga estas instrucciones para crear la aplicación de inicio.</span><span class="sxs-lookup"><span data-stu-id="671bc-278">Follow these instructions to create the starter app.</span></span>

* <span data-ttu-id="671bc-279">Crear un **aplicación Web de ASP.NET Core** con [2017 de Visual Studio](https://www.visualstudio.com/visual-studio-homepage-vs.aspx) denominado "ContactManager"</span><span class="sxs-lookup"><span data-stu-id="671bc-279">Create an **ASP.NET Core Web Application** using [Visual Studio 2017](https://www.visualstudio.com/visual-studio-homepage-vs.aspx) named "ContactManager"</span></span>

  * <span data-ttu-id="671bc-280">Crear la aplicación con **cuentas de usuario individuales**.</span><span class="sxs-lookup"><span data-stu-id="671bc-280">Create the app with **Individual User Accounts**.</span></span>
  * <span data-ttu-id="671bc-281">Asígnele el nombre "ContactManager" para que el espacio de nombres coincidirá con el uso de espacio de nombres en el ejemplo.</span><span class="sxs-lookup"><span data-stu-id="671bc-281">Name it "ContactManager" so your namespace will match the namespace use in the sample.</span></span>

* <span data-ttu-id="671bc-282">Agregue las siguientes `Contact` modelo:</span><span class="sxs-lookup"><span data-stu-id="671bc-282">Add the following `Contact` model:</span></span>

  <span data-ttu-id="671bc-283">[!code-csharp[Main](secure-data/samples/starter/Models/Contact.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="671bc-283">[!code-csharp[Main](secure-data/samples/starter/Models/Contact.cs?name=snippet1)]</span></span>

* <span data-ttu-id="671bc-284">Scaffold el `Contact` modelados con Entity Framework Core y `ApplicationDbContext` contexto de datos.</span><span class="sxs-lookup"><span data-stu-id="671bc-284">Scaffold the `Contact` model using Entity Framework Core and the `ApplicationDbContext` data context.</span></span> <span data-ttu-id="671bc-285">Acepte todos los valores predeterminados de la técnica scaffolding.</span><span class="sxs-lookup"><span data-stu-id="671bc-285">Accept all the scaffolding defaults.</span></span> <span data-ttu-id="671bc-286">Usar `ApplicationDbContext` para el contexto de datos de clase pone la tabla contacto la [identidad](xref:security/authentication/identity) base de datos.</span><span class="sxs-lookup"><span data-stu-id="671bc-286">Using `ApplicationDbContext` for the data context class  puts the contact table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="671bc-287">Vea [agregar un modelo](xref:tutorials/first-mvc-app/adding-model) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="671bc-287">See [Adding a model](xref:tutorials/first-mvc-app/adding-model) for more information.</span></span>

* <span data-ttu-id="671bc-288">Actualización del **ContactManager** fijar en el *Views/Shared/_Layout.cshtml* de archivos de `asp-controller="Home"` a `asp-controller="Contacts"` por lo que al puntear en el **ContactManager** vínculo invoca el controlador de contactos.</span><span class="sxs-lookup"><span data-stu-id="671bc-288">Update the **ContactManager** anchor in the *Views/Shared/_Layout.cshtml* file from `asp-controller="Home"` to `asp-controller="Contacts"` so tapping the **ContactManager** link will invoke the Contacts controller.</span></span> <span data-ttu-id="671bc-289">El marcado original:</span><span class="sxs-lookup"><span data-stu-id="671bc-289">The original markup:</span></span>

```html
   <a asp-area="" asp-controller="Home" asp-action="Index" class="navbar-brand">ContactManager</a>
   ```

<span data-ttu-id="671bc-290">El marcado actualizado:</span><span class="sxs-lookup"><span data-stu-id="671bc-290">The updated markup:</span></span>

```html
   <a asp-area="" asp-controller="Contacts" asp-action="Index" class="navbar-brand">ContactManager</a>
   ```

* <span data-ttu-id="671bc-291">Aplicar la técnica scaffolding la migración inicial y actualizar la base de datos</span><span class="sxs-lookup"><span data-stu-id="671bc-291">Scaffold the initial migration and update the database</span></span>

```none
   dotnet ef migrations add initial
   dotnet ef database update
   ```

* <span data-ttu-id="671bc-292">Probar la aplicación mediante la creación, edición y eliminación de un contacto</span><span class="sxs-lookup"><span data-stu-id="671bc-292">Test the app by creating, editing and deleting a contact</span></span>

### <a name="seed-the-database"></a><span data-ttu-id="671bc-293">Valor de inicialización de la base de datos</span><span class="sxs-lookup"><span data-stu-id="671bc-293">Seed the database</span></span>

<span data-ttu-id="671bc-294">Agregar el `SeedData` clase a la *datos* carpeta.</span><span class="sxs-lookup"><span data-stu-id="671bc-294">Add the `SeedData` class to the *Data* folder.</span></span> <span data-ttu-id="671bc-295">Si ha descargado el ejemplo, puede copiar la *SeedData.cs* del archivo a la *datos* carpeta del proyecto de inicio.</span><span class="sxs-lookup"><span data-stu-id="671bc-295">If you've downloaded the sample, you can copy the *SeedData.cs* file to the *Data* folder of the starter project.</span></span>

<span data-ttu-id="671bc-296">[!code-csharp[Main](secure-data/samples/starter/Data/SeedData.cs)]</span><span class="sxs-lookup"><span data-stu-id="671bc-296">[!code-csharp[Main](secure-data/samples/starter/Data/SeedData.cs)]</span></span>

<span data-ttu-id="671bc-297">Agregue el código resaltado hasta el final de la `Configure` método en el *Startup.cs* archivo:</span><span class="sxs-lookup"><span data-stu-id="671bc-297">Add the highlighted code to the end of the `Configure` method in the *Startup.cs* file:</span></span>

<span data-ttu-id="671bc-298">[!code-csharp[Main](secure-data/samples/starter/Startup.cs?name=Configure&highlight=28-)]</span><span class="sxs-lookup"><span data-stu-id="671bc-298">[!code-csharp[Main](secure-data/samples/starter/Startup.cs?name=Configure&highlight=28-)]</span></span>

<span data-ttu-id="671bc-299">Compruebe que la aplicación había propagado la base de datos.</span><span class="sxs-lookup"><span data-stu-id="671bc-299">Test that the app seeded the database.</span></span> <span data-ttu-id="671bc-300">El método de inicialización no se ejecuta si hay filas en la base de datos de contacto.</span><span class="sxs-lookup"><span data-stu-id="671bc-300">The seed method does not run if there are any rows in the contact DB.</span></span>

### <a name="create-a-class-used-in-the-tutorial"></a><span data-ttu-id="671bc-301">Crear una clase usada en el tutorial</span><span class="sxs-lookup"><span data-stu-id="671bc-301">Create a class used in the tutorial</span></span>

* <span data-ttu-id="671bc-302">Cree una carpeta denominada *autorización*.</span><span class="sxs-lookup"><span data-stu-id="671bc-302">Create a folder named *Authorization*.</span></span>
* <span data-ttu-id="671bc-303">Copia la *Authorization\ContactOperations.cs* de archivos desde el proyecto completado para descargar o copie el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="671bc-303">Copy the *Authorization\ContactOperations.cs* file from the completed project download, or copy the following code:</span></span>

<span data-ttu-id="671bc-304">[!code-csharp[Main](secure-data/samples/final/Authorization/ContactOperations.cs)]</span><span class="sxs-lookup"><span data-stu-id="671bc-304">[!code-csharp[Main](secure-data/samples/final/Authorization/ContactOperations.cs)]</span></span>

<a name=secure-data-add-resources-label></a>

### <a name="additional-resources"></a><span data-ttu-id="671bc-305">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="671bc-305">Additional resources</span></span>

* <span data-ttu-id="671bc-306">[Laboratorio de autorización de ASP.NET Core](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span><span class="sxs-lookup"><span data-stu-id="671bc-306">[ASP.NET Core Authorization Lab](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span></span> <span data-ttu-id="671bc-307">Este laboratorio se explica con más detalle en las características de seguridad introducidas en este tutorial.</span><span class="sxs-lookup"><span data-stu-id="671bc-307">This lab goes into more detail on the security features introduced in this tutorial.</span></span>
* [<span data-ttu-id="671bc-308">Autorización de ASP.NET Core: Simple, rol, basada en notificaciones y personalizado</span><span class="sxs-lookup"><span data-stu-id="671bc-308">Authorization in ASP.NET Core : Simple, role, claims-based and custom</span></span>](index.md)
* [<span data-ttu-id="671bc-309">Autorización personalizada basada en directivas</span><span class="sxs-lookup"><span data-stu-id="671bc-309">Custom Policy-Based Authorization</span></span>](policies.md)
