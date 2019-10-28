---
title: Creación de una aplicación ASP.NET Core con datos de usuario protegidos por autorización
author: rick-anderson
description: Obtenga información sobre cómo crear una aplicación Razor Pages con datos de usuario protegidos por autorización. Incluye HTTPS, autenticación, seguridad, ASP.NET Core identidad.
ms.author: riande
ms.date: 12/18/2018
ms.custom: mvc, seodec18
uid: security/authorization/secure-data
ms.openlocfilehash: 6e2f785a6dc014884f105766686f284cb2685530
ms.sourcegitcommit: 383017d7060a6d58f6a79cf4d7335d5b4b6c5659
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/23/2019
ms.locfileid: "72816154"
---
# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a><span data-ttu-id="1844e-104">Creación de una aplicación ASP.NET Core con datos de usuario protegidos por autorización</span><span class="sxs-lookup"><span data-stu-id="1844e-104">Create an ASP.NET Core app with user data protected by authorization</span></span>

<span data-ttu-id="1844e-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT) y [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="1844e-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="1844e-106">Vea [este PDF](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) para la versión de ASP.net Core MVC.</span><span class="sxs-lookup"><span data-stu-id="1844e-106">See [this PDF](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core MVC version.</span></span> <span data-ttu-id="1844e-107">La versión ASP.NET Core 1,1 de este tutorial se encuentra en [esta](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data) carpeta.</span><span class="sxs-lookup"><span data-stu-id="1844e-107">The ASP.NET Core 1.1 version of this tutorial is in [this](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data) folder.</span></span> <span data-ttu-id="1844e-108">El ejemplo 1,1 ASP.NET Core está en los [ejemplos](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).</span><span class="sxs-lookup"><span data-stu-id="1844e-108">The 1.1 ASP.NET Core sample is in the [samples](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="1844e-109">Vea [este PDF](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_July16_18.pdf)</span><span class="sxs-lookup"><span data-stu-id="1844e-109">See [this pdf](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_July16_18.pdf)</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="1844e-110">En este tutorial se muestra cómo crear una aplicación Web de ASP.NET Core con los datos de usuario protegidos por autorización.</span><span class="sxs-lookup"><span data-stu-id="1844e-110">This tutorial shows how to create an ASP.NET Core web app with user data protected by authorization.</span></span> <span data-ttu-id="1844e-111">Muestra una lista de contactos que los usuarios autenticados (registrados) han creado.</span><span class="sxs-lookup"><span data-stu-id="1844e-111">It displays a list of contacts that authenticated (registered) users have created.</span></span> <span data-ttu-id="1844e-112">Hay tres grupos de seguridad:</span><span class="sxs-lookup"><span data-stu-id="1844e-112">There are three security groups:</span></span>

* <span data-ttu-id="1844e-113">**Los usuarios registrados** pueden ver todos los datos aprobados y pueden editar o eliminar sus propios datos.</span><span class="sxs-lookup"><span data-stu-id="1844e-113">**Registered users** can view all the approved data and can edit/delete their own data.</span></span>
* <span data-ttu-id="1844e-114">Los **administradores** pueden aprobar o rechazar datos de contacto.</span><span class="sxs-lookup"><span data-stu-id="1844e-114">**Managers** can approve or reject contact data.</span></span> <span data-ttu-id="1844e-115">Los usuarios solo pueden ver los contactos aprobados.</span><span class="sxs-lookup"><span data-stu-id="1844e-115">Only approved contacts are visible to users.</span></span>
* <span data-ttu-id="1844e-116">**Los administradores** pueden aprobar o rechazar y editar o eliminar los datos.</span><span class="sxs-lookup"><span data-stu-id="1844e-116">**Administrators** can approve/reject and edit/delete any data.</span></span>

<span data-ttu-id="1844e-117">Las imágenes de este documento no coinciden exactamente con las plantillas más recientes.</span><span class="sxs-lookup"><span data-stu-id="1844e-117">The images in this document don't exactly match the latest templates.</span></span>

<span data-ttu-id="1844e-118">En la siguiente imagen, el usuario Rick (`rick@example.com`) ha iniciado sesión.</span><span class="sxs-lookup"><span data-stu-id="1844e-118">In the following image, user Rick (`rick@example.com`) is signed in.</span></span> <span data-ttu-id="1844e-119">Rick solo puede ver contactos aprobados y **editar**/**eliminar**/**crear nuevos** vínculos para sus contactos.</span><span class="sxs-lookup"><span data-stu-id="1844e-119">Rick can only view approved contacts and **Edit**/**Delete**/**Create New** links for his contacts.</span></span> <span data-ttu-id="1844e-120">Solo el último registro, creado por Rick, muestra los vínculos de **edición** y **eliminación** .</span><span class="sxs-lookup"><span data-stu-id="1844e-120">Only the last record, created by Rick, displays **Edit** and **Delete** links.</span></span> <span data-ttu-id="1844e-121">Otros usuarios no verán el último registro hasta que un administrador o un administrador cambie el estado a "aprobado".</span><span class="sxs-lookup"><span data-stu-id="1844e-121">Other users won't see the last record until a manager or administrator changes the status to "Approved".</span></span>

![Captura de pantalla que muestra Rick con sesión iniciada](secure-data/_static/rick.png)

<span data-ttu-id="1844e-123">En la imagen siguiente, `manager@contoso.com` está conectado y en el rol del administrador:</span><span class="sxs-lookup"><span data-stu-id="1844e-123">In the following image, `manager@contoso.com` is signed in and in the manager's role:</span></span>

![Captura de pantalla que muestra manager@contoso.com sesión iniciada](secure-data/_static/manager1.png)

<span data-ttu-id="1844e-125">La siguiente imagen muestra la vista de detalles de los administradores de un contacto:</span><span class="sxs-lookup"><span data-stu-id="1844e-125">The following image shows the managers details view of a contact:</span></span>

![Vista del administrador de un contacto](secure-data/_static/manager.png)

<span data-ttu-id="1844e-127">Los botones **aprobar** y **rechazar** solo se muestran para administradores y administradores.</span><span class="sxs-lookup"><span data-stu-id="1844e-127">The **Approve** and **Reject** buttons are only displayed for managers and administrators.</span></span>

<span data-ttu-id="1844e-128">En la siguiente imagen, `admin@contoso.com` está conectado y en el rol de administrador:</span><span class="sxs-lookup"><span data-stu-id="1844e-128">In the following image, `admin@contoso.com` is signed in and in the administrator's role:</span></span>

![Captura de pantalla que muestra admin@contoso.com sesión iniciada](secure-data/_static/admin.png)

<span data-ttu-id="1844e-130">El administrador tiene todos los privilegios.</span><span class="sxs-lookup"><span data-stu-id="1844e-130">The administrator has all privileges.</span></span> <span data-ttu-id="1844e-131">Puede leer, modificar o eliminar cualquier contacto y cambiar el estado de los contactos.</span><span class="sxs-lookup"><span data-stu-id="1844e-131">She can read/edit/delete any contact and change the status of contacts.</span></span>

<span data-ttu-id="1844e-132">La aplicación se creó mediante [scaffolding](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model) en el siguiente modelo de `Contact`:</span><span class="sxs-lookup"><span data-stu-id="1844e-132">The app was created by [scaffolding](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model) the following `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

<span data-ttu-id="1844e-133">El ejemplo contiene los siguientes controladores de autorización:</span><span class="sxs-lookup"><span data-stu-id="1844e-133">The sample contains the following authorization handlers:</span></span>

* <span data-ttu-id="1844e-134">`ContactIsOwnerAuthorizationHandler`: garantiza que un usuario solo pueda editar sus datos.</span><span class="sxs-lookup"><span data-stu-id="1844e-134">`ContactIsOwnerAuthorizationHandler`: Ensures that a user can only edit their data.</span></span>
* <span data-ttu-id="1844e-135">`ContactManagerAuthorizationHandler`: permite a los administradores aprobar o rechazar contactos.</span><span class="sxs-lookup"><span data-stu-id="1844e-135">`ContactManagerAuthorizationHandler`: Allows managers to approve or reject contacts.</span></span>
* <span data-ttu-id="1844e-136">`ContactAdministratorsAuthorizationHandler`: permite a los administradores aprobar o rechazar contactos y editar o eliminar contactos.</span><span class="sxs-lookup"><span data-stu-id="1844e-136">`ContactAdministratorsAuthorizationHandler`: Allows administrators to approve or reject contacts and to edit/delete contacts.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1844e-137">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="1844e-137">Prerequisites</span></span>

<span data-ttu-id="1844e-138">Este tutorial es avanzado.</span><span class="sxs-lookup"><span data-stu-id="1844e-138">This tutorial is advanced.</span></span> <span data-ttu-id="1844e-139">Debe estar familiarizado con:</span><span class="sxs-lookup"><span data-stu-id="1844e-139">You should be familiar with:</span></span>

* [<span data-ttu-id="1844e-140">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1844e-140">ASP.NET Core</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="1844e-141">Autenticación</span><span class="sxs-lookup"><span data-stu-id="1844e-141">Authentication</span></span>](xref:security/authentication/identity)
* [<span data-ttu-id="1844e-142">Confirmación de las cuentas y recuperación de contraseñas</span><span class="sxs-lookup"><span data-stu-id="1844e-142">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="1844e-143">Autorización</span><span class="sxs-lookup"><span data-stu-id="1844e-143">Authorization</span></span>](xref:security/authorization/introduction)
* [<span data-ttu-id="1844e-144">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="1844e-144">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

## <a name="the-starter-and-completed-app"></a><span data-ttu-id="1844e-145">La aplicación de inicio y completada</span><span class="sxs-lookup"><span data-stu-id="1844e-145">The starter and completed app</span></span>

<span data-ttu-id="1844e-146">[Descargue](xref:index#how-to-download-a-sample) la aplicación [completada](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples) .</span><span class="sxs-lookup"><span data-stu-id="1844e-146">[Download](xref:index#how-to-download-a-sample) the [completed](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples) app.</span></span> <span data-ttu-id="1844e-147">[Pruebe](#test-the-completed-app) la aplicación completada para familiarizarse con sus características de seguridad.</span><span class="sxs-lookup"><span data-stu-id="1844e-147">[Test](#test-the-completed-app) the completed app so you become familiar with its security features.</span></span>

### <a name="the-starter-app"></a><span data-ttu-id="1844e-148">La aplicación de inicio</span><span class="sxs-lookup"><span data-stu-id="1844e-148">The starter app</span></span>

<span data-ttu-id="1844e-149">[Descargue](xref:index#how-to-download-a-sample) la aplicación de [Inicio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/) .</span><span class="sxs-lookup"><span data-stu-id="1844e-149">[Download](xref:index#how-to-download-a-sample) the [starter](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/) app.</span></span>

<span data-ttu-id="1844e-150">Ejecute la aplicación, pulse el vínculo **ContactManager** y compruebe que puede crear, editar y eliminar un contacto.</span><span class="sxs-lookup"><span data-stu-id="1844e-150">Run the app, tap the **ContactManager** link, and verify you can create, edit, and delete a contact.</span></span>

## <a name="secure-user-data"></a><span data-ttu-id="1844e-151">Proteger los datos de usuario</span><span class="sxs-lookup"><span data-stu-id="1844e-151">Secure user data</span></span>

<span data-ttu-id="1844e-152">En las secciones siguientes se describen todos los pasos principales para crear la aplicación de datos de usuario segura.</span><span class="sxs-lookup"><span data-stu-id="1844e-152">The following sections have all the major steps to create the secure user data app.</span></span> <span data-ttu-id="1844e-153">Puede que le resulte útil consultar el proyecto completado.</span><span class="sxs-lookup"><span data-stu-id="1844e-153">You may find it helpful to refer to the completed project.</span></span>

### <a name="tie-the-contact-data-to-the-user"></a><span data-ttu-id="1844e-154">Vincular los datos de contacto al usuario</span><span class="sxs-lookup"><span data-stu-id="1844e-154">Tie the contact data to the user</span></span>

<span data-ttu-id="1844e-155">Use el identificador de usuario de [identidad](xref:security/authentication/identity) de ASP.net para asegurarse de que los usuarios puedan editar sus datos, pero no los datos de otros usuarios.</span><span class="sxs-lookup"><span data-stu-id="1844e-155">Use the ASP.NET [Identity](xref:security/authentication/identity) user ID to ensure users can edit their data, but not other users data.</span></span> <span data-ttu-id="1844e-156">Agregue `OwnerID` y `ContactStatus` al modelo de `Contact`:</span><span class="sxs-lookup"><span data-stu-id="1844e-156">Add `OwnerID` and `ContactStatus` to the `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/final3/Models/Contact.cs?name=snippet1&highlight=5-6,16-999)]

<span data-ttu-id="1844e-157">`OwnerID` es el identificador del usuario de la tabla de `AspNetUser` de la base de datos de [identidad](xref:security/authentication/identity) .</span><span class="sxs-lookup"><span data-stu-id="1844e-157">`OwnerID` is the user's ID from the `AspNetUser` table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="1844e-158">El campo `Status` determina si los usuarios generales pueden ver un contacto.</span><span class="sxs-lookup"><span data-stu-id="1844e-158">The `Status` field determines if a contact is viewable by general users.</span></span>

<span data-ttu-id="1844e-159">Cree una nueva migración y actualice la base de datos:</span><span class="sxs-lookup"><span data-stu-id="1844e-159">Create a new migration and update the database:</span></span>

```dotnetcli
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="add-role-services-to-identity"></a><span data-ttu-id="1844e-160">Agregar servicios de función a la identidad</span><span class="sxs-lookup"><span data-stu-id="1844e-160">Add Role services to Identity</span></span>

<span data-ttu-id="1844e-161">Anexe [AddRoles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) para agregar servicios de rol:</span><span class="sxs-lookup"><span data-stu-id="1844e-161">Append [AddRoles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) to add Role services:</span></span>

[!code-csharp[](secure-data/samples/final3/Startup.cs?name=snippet2&highlight=9)]

### <a name="require-authenticated-users"></a><span data-ttu-id="1844e-162">Requerir usuarios autenticados</span><span class="sxs-lookup"><span data-stu-id="1844e-162">Require authenticated users</span></span>

<span data-ttu-id="1844e-163">Establezca la Directiva de autenticación predeterminada para requerir la autenticación de los usuarios:</span><span class="sxs-lookup"><span data-stu-id="1844e-163">Set the default authentication policy to require users to be authenticated:</span></span>

[!code-csharp[](secure-data/samples/final3/Startup.cs?name=snippet&highlight=15-99)] 

 <span data-ttu-id="1844e-164">Puede rechazar la autenticación en el nivel de la página de Razor, el controlador o el método de acción con el atributo `[AllowAnonymous]`.</span><span class="sxs-lookup"><span data-stu-id="1844e-164">You can opt out of authentication at the Razor Page, controller, or action method level with the `[AllowAnonymous]` attribute.</span></span> <span data-ttu-id="1844e-165">La configuración de la Directiva de autenticación predeterminada para requerir que los usuarios se autentiquen protege los controladores y Razor Pages recién agregados.</span><span class="sxs-lookup"><span data-stu-id="1844e-165">Setting the default authentication policy to require users to be authenticated protects newly added Razor Pages and controllers.</span></span> <span data-ttu-id="1844e-166">Tener autenticación necesaria de forma predeterminada es más seguro que confiar en los nuevos controladores y Razor Pages para incluir el atributo `[Authorize]`.</span><span class="sxs-lookup"><span data-stu-id="1844e-166">Having authentication required by default is more secure than relying on new controllers and Razor Pages to include the `[Authorize]` attribute.</span></span>

<span data-ttu-id="1844e-167">Agregue [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) a las páginas de índice y privacidad para que los usuarios anónimos puedan obtener información sobre el sitio antes de registrarse.</span><span class="sxs-lookup"><span data-stu-id="1844e-167">Add [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) to the Index and Privacy pages so anonymous users can get information about the site before they register.</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Index.cshtml.cs?highlight=1,7)]

### <a name="configure-the-test-account"></a><span data-ttu-id="1844e-168">Configurar la cuenta de prueba</span><span class="sxs-lookup"><span data-stu-id="1844e-168">Configure the test account</span></span>

<span data-ttu-id="1844e-169">La clase `SeedData` crea dos cuentas: administrador y administrador.</span><span class="sxs-lookup"><span data-stu-id="1844e-169">The `SeedData` class creates two accounts: administrator and manager.</span></span> <span data-ttu-id="1844e-170">Use la [herramienta Administrador de secretos](xref:security/app-secrets) para establecer una contraseña para estas cuentas.</span><span class="sxs-lookup"><span data-stu-id="1844e-170">Use the [Secret Manager tool](xref:security/app-secrets) to set a password for these accounts.</span></span> <span data-ttu-id="1844e-171">Establezca la contraseña desde el directorio del proyecto (el directorio que contiene *Program.CS*):</span><span class="sxs-lookup"><span data-stu-id="1844e-171">Set the password from the project directory (the directory containing *Program.cs*):</span></span>

```dotnetcli
dotnet user-secrets set SeedUserPW <PW>
```

<span data-ttu-id="1844e-172">Si no se especifica una contraseña segura, se produce una excepción cuando se llama a `SeedData.Initialize`.</span><span class="sxs-lookup"><span data-stu-id="1844e-172">If a strong password is not specified, an exception is thrown when `SeedData.Initialize` is called.</span></span>

<span data-ttu-id="1844e-173">Actualice `Main` para usar la contraseña de prueba:</span><span class="sxs-lookup"><span data-stu-id="1844e-173">Update `Main` to use the test password:</span></span>

[!code-csharp[](secure-data/samples/final3/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a><span data-ttu-id="1844e-174">Crear las cuentas de prueba y actualizar los contactos</span><span class="sxs-lookup"><span data-stu-id="1844e-174">Create the test accounts and update the contacts</span></span>

<span data-ttu-id="1844e-175">Actualice el método `Initialize` de la clase `SeedData` para crear las cuentas de prueba:</span><span class="sxs-lookup"><span data-stu-id="1844e-175">Update the `Initialize` method in the `SeedData` class to create the test accounts:</span></span>

[!code-csharp[](secure-data/samples/final3/Data/SeedData.cs?name=snippet_Initialize)]

<span data-ttu-id="1844e-176">Agregue el identificador de usuario de administrador y el `ContactStatus` a los contactos.</span><span class="sxs-lookup"><span data-stu-id="1844e-176">Add the administrator user ID and `ContactStatus` to the contacts.</span></span> <span data-ttu-id="1844e-177">Haga que uno de los contactos "enviado" y otro "rechazado".</span><span class="sxs-lookup"><span data-stu-id="1844e-177">Make one of the contacts "Submitted" and one "Rejected".</span></span> <span data-ttu-id="1844e-178">Agregue el identificador de usuario y el estado a todos los contactos.</span><span class="sxs-lookup"><span data-stu-id="1844e-178">Add the user ID and status to all the contacts.</span></span> <span data-ttu-id="1844e-179">Solo se muestra un contacto:</span><span class="sxs-lookup"><span data-stu-id="1844e-179">Only one contact is shown:</span></span>

[!code-csharp[](secure-data/samples/final3/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a><span data-ttu-id="1844e-180">Crear controladores de autorización de propietario, administrador y administrador</span><span class="sxs-lookup"><span data-stu-id="1844e-180">Create owner, manager, and administrator authorization handlers</span></span>

<span data-ttu-id="1844e-181">Cree una clase de `ContactIsOwnerAuthorizationHandler` en la carpeta de *autorización* .</span><span class="sxs-lookup"><span data-stu-id="1844e-181">Create a `ContactIsOwnerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="1844e-182">En el `ContactIsOwnerAuthorizationHandler` se comprueba que el usuario que actúa en un recurso posee el recurso.</span><span class="sxs-lookup"><span data-stu-id="1844e-182">The `ContactIsOwnerAuthorizationHandler` verifies that the user acting on a resource owns the resource.</span></span>

[!code-csharp[](secure-data/samples/final3/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

<span data-ttu-id="1844e-183">El `ContactIsOwnerAuthorizationHandler` llama al [contexto. Se realiza correctamente](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) si el usuario autenticado actual es el propietario del contacto.</span><span class="sxs-lookup"><span data-stu-id="1844e-183">The `ContactIsOwnerAuthorizationHandler` calls [context.Succeed](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) if the current authenticated user is the contact owner.</span></span> <span data-ttu-id="1844e-184">Los controladores de autorización generalmente:</span><span class="sxs-lookup"><span data-stu-id="1844e-184">Authorization handlers generally:</span></span>

* <span data-ttu-id="1844e-185">Devuelve `context.Succeed` cuando se cumplen los requisitos.</span><span class="sxs-lookup"><span data-stu-id="1844e-185">Return `context.Succeed` when the requirements are met.</span></span>
* <span data-ttu-id="1844e-186">Devuelve `Task.CompletedTask` cuando no se cumplen los requisitos.</span><span class="sxs-lookup"><span data-stu-id="1844e-186">Return `Task.CompletedTask` when requirements aren't met.</span></span> <span data-ttu-id="1844e-187">`Task.CompletedTask` no es correcto o error&mdash;permite que se ejecuten otros controladores de autorización.</span><span class="sxs-lookup"><span data-stu-id="1844e-187">`Task.CompletedTask` is not success or failure&mdash;it allows other authorization handlers to run.</span></span>

<span data-ttu-id="1844e-188">Si necesita generar un error explícitamente, devuelva el [contexto. Error](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span><span class="sxs-lookup"><span data-stu-id="1844e-188">If you need to explicitly fail, return [context.Fail](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span></span>

<span data-ttu-id="1844e-189">La aplicación permite que los propietarios de contactos editen o eliminen o creen sus propios datos.</span><span class="sxs-lookup"><span data-stu-id="1844e-189">The app allows contact owners to edit/delete/create their own data.</span></span> <span data-ttu-id="1844e-190">`ContactIsOwnerAuthorizationHandler` no necesita comprobar la operación que se ha pasado en el parámetro de requisito.</span><span class="sxs-lookup"><span data-stu-id="1844e-190">`ContactIsOwnerAuthorizationHandler` doesn't need to check the operation passed in the requirement parameter.</span></span>

### <a name="create-a-manager-authorization-handler"></a><span data-ttu-id="1844e-191">Crear un controlador de autorización de administrador</span><span class="sxs-lookup"><span data-stu-id="1844e-191">Create a manager authorization handler</span></span>

<span data-ttu-id="1844e-192">Cree una clase de `ContactManagerAuthorizationHandler` en la carpeta de *autorización* .</span><span class="sxs-lookup"><span data-stu-id="1844e-192">Create a `ContactManagerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="1844e-193">El `ContactManagerAuthorizationHandler` comprueba que el usuario que actúa en el recurso es un administrador.</span><span class="sxs-lookup"><span data-stu-id="1844e-193">The `ContactManagerAuthorizationHandler` verifies the user acting on the resource is a manager.</span></span> <span data-ttu-id="1844e-194">Solo los administradores pueden aprobar o rechazar cambios de contenido (nuevos o modificados).</span><span class="sxs-lookup"><span data-stu-id="1844e-194">Only managers can approve or reject content changes (new or changed).</span></span>

[!code-csharp[](secure-data/samples/final3/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a><span data-ttu-id="1844e-195">Crear un controlador de autorización de administrador</span><span class="sxs-lookup"><span data-stu-id="1844e-195">Create an administrator authorization handler</span></span>

<span data-ttu-id="1844e-196">Cree una clase de `ContactAdministratorsAuthorizationHandler` en la carpeta de *autorización* .</span><span class="sxs-lookup"><span data-stu-id="1844e-196">Create a `ContactAdministratorsAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="1844e-197">El `ContactAdministratorsAuthorizationHandler` comprueba que el usuario que actúa en el recurso es un administrador.</span><span class="sxs-lookup"><span data-stu-id="1844e-197">The `ContactAdministratorsAuthorizationHandler` verifies the user acting on the resource is an administrator.</span></span> <span data-ttu-id="1844e-198">El administrador puede realizar todas las operaciones.</span><span class="sxs-lookup"><span data-stu-id="1844e-198">Administrator can do all operations.</span></span>

[!code-csharp[](secure-data/samples/final3/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a><span data-ttu-id="1844e-199">Registrar los controladores de autorización</span><span class="sxs-lookup"><span data-stu-id="1844e-199">Register the authorization handlers</span></span>

<span data-ttu-id="1844e-200">Los servicios que usan Entity Framework Core deben estar registrados para la [inserción de dependencias](xref:fundamentals/dependency-injection) mediante [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span><span class="sxs-lookup"><span data-stu-id="1844e-200">Services using Entity Framework Core must be registered for [dependency injection](xref:fundamentals/dependency-injection) using [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span></span> <span data-ttu-id="1844e-201">En el `ContactIsOwnerAuthorizationHandler` se usa ASP.NET Core [Identity](xref:security/authentication/identity), que se basa en Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="1844e-201">The `ContactIsOwnerAuthorizationHandler` uses ASP.NET Core [Identity](xref:security/authentication/identity), which is built on Entity Framework Core.</span></span> <span data-ttu-id="1844e-202">Registre los controladores con la colección de servicios para que estén disponibles para el `ContactsController` a través de la [inserción de dependencias](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="1844e-202">Register the handlers with the service collection so they're available to the `ContactsController` through [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="1844e-203">Agregue el código siguiente al final de `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="1844e-203">Add the following code to the end of `ConfigureServices`:</span></span>

[!code-csharp[](secure-data/samples/final3/Startup.cs?name=snippet_defaultPolicy&highlight=23-99)]

<span data-ttu-id="1844e-204">`ContactAdministratorsAuthorizationHandler` y `ContactManagerAuthorizationHandler` se agregan como singletons.</span><span class="sxs-lookup"><span data-stu-id="1844e-204">`ContactAdministratorsAuthorizationHandler` and `ContactManagerAuthorizationHandler` are added as singletons.</span></span> <span data-ttu-id="1844e-205">Son singleton porque no usan EF y toda la información necesaria está en el parámetro `Context` del método `HandleRequirementAsync`.</span><span class="sxs-lookup"><span data-stu-id="1844e-205">They're singletons because they don't use EF and all the information needed is in the `Context` parameter of the `HandleRequirementAsync` method.</span></span>

## <a name="support-authorization"></a><span data-ttu-id="1844e-206">Autorización de soporte técnico</span><span class="sxs-lookup"><span data-stu-id="1844e-206">Support authorization</span></span>

<span data-ttu-id="1844e-207">En esta sección, actualizará el Razor Pages y agregará una clase de requisitos de operaciones.</span><span class="sxs-lookup"><span data-stu-id="1844e-207">In this section, you update the Razor Pages and add an operations requirements class.</span></span>

### <a name="review-the-contact-operations-requirements-class"></a><span data-ttu-id="1844e-208">Revisar la clase de requisitos de las operaciones de contacto</span><span class="sxs-lookup"><span data-stu-id="1844e-208">Review the contact operations requirements class</span></span>

<span data-ttu-id="1844e-209">Revise la clase `ContactOperations`.</span><span class="sxs-lookup"><span data-stu-id="1844e-209">Review the `ContactOperations` class.</span></span> <span data-ttu-id="1844e-210">Esta clase contiene los requisitos que admite la aplicación:</span><span class="sxs-lookup"><span data-stu-id="1844e-210">This class contains the requirements the app supports:</span></span>

[!code-csharp[](secure-data/samples/final3/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-contacts-razor-pages"></a><span data-ttu-id="1844e-211">Cree una clase base para el Razor Pages contactos</span><span class="sxs-lookup"><span data-stu-id="1844e-211">Create a base class for the Contacts Razor Pages</span></span>

<span data-ttu-id="1844e-212">Cree una clase base que contenga los servicios usados en el Razor Pages contactos.</span><span class="sxs-lookup"><span data-stu-id="1844e-212">Create a base class that contains the services used in the contacts Razor Pages.</span></span> <span data-ttu-id="1844e-213">La clase base coloca el código de inicialización en una ubicación:</span><span class="sxs-lookup"><span data-stu-id="1844e-213">The base class puts the initialization code in one location:</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/DI_BasePageModel.cs)]

<span data-ttu-id="1844e-214">El código anterior:</span><span class="sxs-lookup"><span data-stu-id="1844e-214">The preceding code:</span></span>

* <span data-ttu-id="1844e-215">Agrega el servicio `IAuthorizationService` para tener acceso a los controladores de autorización.</span><span class="sxs-lookup"><span data-stu-id="1844e-215">Adds the `IAuthorizationService` service to access to the authorization handlers.</span></span>
* <span data-ttu-id="1844e-216">Agrega el servicio Identity `UserManager`.</span><span class="sxs-lookup"><span data-stu-id="1844e-216">Adds the Identity `UserManager` service.</span></span>
* <span data-ttu-id="1844e-217">Agregue la `ApplicationDbContext`.</span><span class="sxs-lookup"><span data-stu-id="1844e-217">Add the `ApplicationDbContext`.</span></span>

### <a name="update-the-createmodel"></a><span data-ttu-id="1844e-218">Actualización de CreateModel</span><span class="sxs-lookup"><span data-stu-id="1844e-218">Update the CreateModel</span></span>

<span data-ttu-id="1844e-219">Actualice el constructor Create Page Model para usar la clase base `DI_BasePageModel`:</span><span class="sxs-lookup"><span data-stu-id="1844e-219">Update the create page model constructor to use the `DI_BasePageModel` base class:</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

<span data-ttu-id="1844e-220">Actualice el método `CreateModel.OnPostAsync` para:</span><span class="sxs-lookup"><span data-stu-id="1844e-220">Update the `CreateModel.OnPostAsync` method to:</span></span>

* <span data-ttu-id="1844e-221">Agregue el identificador de usuario al modelo de `Contact`.</span><span class="sxs-lookup"><span data-stu-id="1844e-221">Add the user ID to the `Contact` model.</span></span>
* <span data-ttu-id="1844e-222">Llame al controlador de autorización para comprobar que el usuario tiene permiso para crear contactos.</span><span class="sxs-lookup"><span data-stu-id="1844e-222">Call the authorization handler to verify the user has permission to create contacts.</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a><span data-ttu-id="1844e-223">Actualización de IndexModel</span><span class="sxs-lookup"><span data-stu-id="1844e-223">Update the IndexModel</span></span>

<span data-ttu-id="1844e-224">Actualice el método de `OnGetAsync` de modo que solo se muestren los contactos aprobados a los usuarios generales:</span><span class="sxs-lookup"><span data-stu-id="1844e-224">Update the `OnGetAsync` method so only approved contacts are shown to general users:</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a><span data-ttu-id="1844e-225">Actualización de EditModel</span><span class="sxs-lookup"><span data-stu-id="1844e-225">Update the EditModel</span></span>

<span data-ttu-id="1844e-226">Agregue un controlador de autorización para comprobar que el usuario es el propietario del contacto.</span><span class="sxs-lookup"><span data-stu-id="1844e-226">Add an authorization handler to verify the user owns the contact.</span></span> <span data-ttu-id="1844e-227">Dado que se está validando la autorización de recursos, el atributo `[Authorize]` no es suficiente.</span><span class="sxs-lookup"><span data-stu-id="1844e-227">Because resource authorization is being validated, the `[Authorize]` attribute is not enough.</span></span> <span data-ttu-id="1844e-228">La aplicación no tiene acceso al recurso cuando se evalúan los atributos.</span><span class="sxs-lookup"><span data-stu-id="1844e-228">The app doesn't have access to the resource when attributes are evaluated.</span></span> <span data-ttu-id="1844e-229">La autorización basada en recursos debe ser imperativa.</span><span class="sxs-lookup"><span data-stu-id="1844e-229">Resource-based authorization must be imperative.</span></span> <span data-ttu-id="1844e-230">Las comprobaciones deben realizarse una vez que la aplicación tenga acceso al recurso, ya sea mediante su carga en el modelo de página o mediante su carga en el propio controlador.</span><span class="sxs-lookup"><span data-stu-id="1844e-230">Checks must be performed once the app has access to the resource, either by loading it in the page model or by loading it within the handler itself.</span></span> <span data-ttu-id="1844e-231">Frecuentemente tiene acceso al recurso pasando la clave de recurso.</span><span class="sxs-lookup"><span data-stu-id="1844e-231">You frequently access the resource by passing in the resource key.</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a><span data-ttu-id="1844e-232">Actualización de DeleteModel</span><span class="sxs-lookup"><span data-stu-id="1844e-232">Update the DeleteModel</span></span>

<span data-ttu-id="1844e-233">Actualice el modelo de página de eliminación para que use el controlador de autorización para comprobar que el usuario tiene el permiso eliminar en el contacto.</span><span class="sxs-lookup"><span data-stu-id="1844e-233">Update the delete page model to use the authorization handler to verify the user has delete permission on the contact.</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a><span data-ttu-id="1844e-234">Inyectar el servicio de autorización en las vistas</span><span class="sxs-lookup"><span data-stu-id="1844e-234">Inject the authorization service into the views</span></span>

<span data-ttu-id="1844e-235">Actualmente, la interfaz de usuario muestra vínculos de edición y eliminación para contactos que el usuario no puede modificar.</span><span class="sxs-lookup"><span data-stu-id="1844e-235">Currently, the UI shows edit and delete links for contacts the user can't modify.</span></span>

<span data-ttu-id="1844e-236">Inserte el servicio de autorización en el archivo *pages/_ViewImports. cshtml* para que esté disponible para todas las vistas:</span><span class="sxs-lookup"><span data-stu-id="1844e-236">Inject the authorization service in the *Pages/_ViewImports.cshtml* file so it's available to all views:</span></span>

[!code-cshtml[](secure-data/samples/final3/Pages/_ViewImports.cshtml?highlight=6-99)]

<span data-ttu-id="1844e-237">El marcado anterior agrega varias instrucciones de `using`.</span><span class="sxs-lookup"><span data-stu-id="1844e-237">The preceding markup adds several `using` statements.</span></span>

<span data-ttu-id="1844e-238">Actualice los vínculos de **edición** y **eliminación** en *pages/Contacts/index. cshtml* para que solo se representen para los usuarios con los permisos adecuados:</span><span class="sxs-lookup"><span data-stu-id="1844e-238">Update the **Edit** and **Delete** links in *Pages/Contacts/Index.cshtml* so they're only rendered for users with the appropriate permissions:</span></span>

[!code-cshtml[](secure-data/samples/final3/Pages/Contacts/Index.cshtml?highlight=34-36,62-999)]

> [!WARNING]
> <span data-ttu-id="1844e-239">Ocultar los vínculos de los usuarios que no tienen permiso para cambiar datos no protege la aplicación.</span><span class="sxs-lookup"><span data-stu-id="1844e-239">Hiding links from users that don't have permission to change data doesn't secure the app.</span></span> <span data-ttu-id="1844e-240">La ocultación de los vínculos hace que la aplicación sea más fácil de ver mostrando solo vínculos válidos.</span><span class="sxs-lookup"><span data-stu-id="1844e-240">Hiding links makes the app more user-friendly by displaying only valid links.</span></span> <span data-ttu-id="1844e-241">Los usuarios pueden atacar las direcciones URL generadas para invocar operaciones de edición y eliminación en los datos que no son de su propiedad.</span><span class="sxs-lookup"><span data-stu-id="1844e-241">Users can hack the generated URLs to invoke edit and delete operations on data they don't own.</span></span> <span data-ttu-id="1844e-242">La página o el controlador de Razor deben aplicar las comprobaciones de acceso para proteger los datos.</span><span class="sxs-lookup"><span data-stu-id="1844e-242">The Razor Page or controller must enforce access checks to secure the data.</span></span>

### <a name="update-details"></a><span data-ttu-id="1844e-243">Detalles de la actualización</span><span class="sxs-lookup"><span data-stu-id="1844e-243">Update Details</span></span>

<span data-ttu-id="1844e-244">Actualice la vista de detalles para que los administradores puedan aprobar o rechazar contactos:</span><span class="sxs-lookup"><span data-stu-id="1844e-244">Update the details view so managers can approve or reject contacts:</span></span>

[!code-cshtml[](secure-data/samples/final3/Pages/Contacts/Details.cshtml?name=snippet)]

<span data-ttu-id="1844e-245">Actualice el modelo de página de detalles:</span><span class="sxs-lookup"><span data-stu-id="1844e-245">Update the details page model:</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="add-or-remove-a-user-to-a-role"></a><span data-ttu-id="1844e-246">Agregar o quitar un usuario de un rol</span><span class="sxs-lookup"><span data-stu-id="1844e-246">Add or remove a user to a role</span></span>

<span data-ttu-id="1844e-247">Consulte [este problema](https://github.com/aspnet/AspNetCore.Docs/issues/8502) para obtener información sobre:</span><span class="sxs-lookup"><span data-stu-id="1844e-247">See [this issue](https://github.com/aspnet/AspNetCore.Docs/issues/8502) for information on:</span></span>

* <span data-ttu-id="1844e-248">Quitar privilegios de un usuario.</span><span class="sxs-lookup"><span data-stu-id="1844e-248">Removing privileges from a user.</span></span> <span data-ttu-id="1844e-249">Por ejemplo, silenciar a un usuario en una aplicación de chat.</span><span class="sxs-lookup"><span data-stu-id="1844e-249">For example, muting a user in a chat app.</span></span>
* <span data-ttu-id="1844e-250">Agregar privilegios a un usuario.</span><span class="sxs-lookup"><span data-stu-id="1844e-250">Adding privileges to a user.</span></span>

## <a name="differences-between-challenge-vs-forbid"></a><span data-ttu-id="1844e-251">Diferencias entre desafío y prohibido</span><span class="sxs-lookup"><span data-stu-id="1844e-251">Differences between Challenge vs Forbid</span></span>

<span data-ttu-id="1844e-252">Esta aplicación establece la directiva predeterminada para [requerir usuarios autenticados](#require-authenticated-users).</span><span class="sxs-lookup"><span data-stu-id="1844e-252">This app sets the default policy to [require authenticated users](#require-authenticated-users).</span></span> <span data-ttu-id="1844e-253">El código siguiente permite usuarios anónimos.</span><span class="sxs-lookup"><span data-stu-id="1844e-253">The following code allows anonymous users.</span></span> <span data-ttu-id="1844e-254">Los usuarios anónimos pueden mostrar las diferencias entre desafío y prohibido.</span><span class="sxs-lookup"><span data-stu-id="1844e-254">Anonymous users are allowed to show the differences between Challenge vs Forbid.</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Details2.cshtml.cs?name=snippet)]

<span data-ttu-id="1844e-255">En el código anterior:</span><span class="sxs-lookup"><span data-stu-id="1844e-255">In the preceding code:</span></span>

* <span data-ttu-id="1844e-256">Cuando el usuario **no** está autenticado, se devuelve un `ChallengeResult`.</span><span class="sxs-lookup"><span data-stu-id="1844e-256">When the user is **not** authenticated, a `ChallengeResult` is returned.</span></span> <span data-ttu-id="1844e-257">Cuando se devuelve un `ChallengeResult`, se redirige al usuario a la página de inicio de sesión.</span><span class="sxs-lookup"><span data-stu-id="1844e-257">When a `ChallengeResult` is returned, the user is redirected to the sign-in page.</span></span>
* <span data-ttu-id="1844e-258">Cuando el usuario está autenticado, pero no autorizado, se devuelve un `ForbidResult`.</span><span class="sxs-lookup"><span data-stu-id="1844e-258">When the user is authenticated, but not authorized, a `ForbidResult` is returned.</span></span> <span data-ttu-id="1844e-259">Cuando se devuelve un `ForbidResult`, se redirige al usuario a la página acceso denegado.</span><span class="sxs-lookup"><span data-stu-id="1844e-259">When a `ForbidResult` is returned, the user is redirected to the access denied page.</span></span>

## <a name="test-the-completed-app"></a><span data-ttu-id="1844e-260">Prueba de la aplicación completada</span><span class="sxs-lookup"><span data-stu-id="1844e-260">Test the completed app</span></span>

<span data-ttu-id="1844e-261">Si aún no ha establecido una contraseña para las cuentas de usuario inicializadas, use la [herramienta Administrador de secretos](xref:security/app-secrets#secret-manager) para establecer una contraseña:</span><span class="sxs-lookup"><span data-stu-id="1844e-261">If you haven't already set a password for seeded user accounts, use the [Secret Manager tool](xref:security/app-secrets#secret-manager) to set a password:</span></span>

* <span data-ttu-id="1844e-262">Elija una contraseña segura: Use ocho o más caracteres y, al menos, un carácter en mayúscula, un número y un símbolo.</span><span class="sxs-lookup"><span data-stu-id="1844e-262">Choose a strong password: Use eight or more characters and at least one upper-case character, number, and symbol.</span></span> <span data-ttu-id="1844e-263">Por ejemplo, `Passw0rd!` cumple los requisitos de contraseñas seguras.</span><span class="sxs-lookup"><span data-stu-id="1844e-263">For example, `Passw0rd!` meets the strong password requirements.</span></span>
* <span data-ttu-id="1844e-264">Ejecute el siguiente comando desde la carpeta del proyecto, donde `<PW>` es la contraseña:</span><span class="sxs-lookup"><span data-stu-id="1844e-264">Execute the following command from the project's folder, where `<PW>` is the password:</span></span>

  ```dotnetcli
  dotnet user-secrets set SeedUserPW <PW>
  ```

<span data-ttu-id="1844e-265">Si la aplicación tiene contactos:</span><span class="sxs-lookup"><span data-stu-id="1844e-265">If the app has contacts:</span></span>

* <span data-ttu-id="1844e-266">Elimine todos los registros de la tabla `Contact`.</span><span class="sxs-lookup"><span data-stu-id="1844e-266">Delete all of the records in the `Contact` table.</span></span>
* <span data-ttu-id="1844e-267">Reinicie la aplicación para inicializar la base de datos.</span><span class="sxs-lookup"><span data-stu-id="1844e-267">Restart the app to seed the database.</span></span>

<span data-ttu-id="1844e-268">Una manera fácil de probar la aplicación completada es iniciar tres exploradores diferentes (o sesiones de incógnito/InPrivate).</span><span class="sxs-lookup"><span data-stu-id="1844e-268">An easy way to test the completed app is to launch three different browsers (or incognito/InPrivate sessions).</span></span> <span data-ttu-id="1844e-269">En un explorador, registre un nuevo usuario (por ejemplo, `test@example.com`).</span><span class="sxs-lookup"><span data-stu-id="1844e-269">In one browser, register a new user (for example, `test@example.com`).</span></span> <span data-ttu-id="1844e-270">Inicie sesión en cada explorador con un usuario diferente.</span><span class="sxs-lookup"><span data-stu-id="1844e-270">Sign in to each browser with a different user.</span></span> <span data-ttu-id="1844e-271">Compruebe las siguientes operaciones:</span><span class="sxs-lookup"><span data-stu-id="1844e-271">Verify the following operations:</span></span>

* <span data-ttu-id="1844e-272">Los usuarios registrados pueden ver todos los datos de contacto aprobados.</span><span class="sxs-lookup"><span data-stu-id="1844e-272">Registered users can view all of the approved contact data.</span></span>
* <span data-ttu-id="1844e-273">Los usuarios registrados pueden editar o eliminar sus propios datos.</span><span class="sxs-lookup"><span data-stu-id="1844e-273">Registered users can edit/delete their own data.</span></span>
* <span data-ttu-id="1844e-274">Los administradores pueden aprobar o rechazar datos de contacto.</span><span class="sxs-lookup"><span data-stu-id="1844e-274">Managers can approve/reject contact data.</span></span> <span data-ttu-id="1844e-275">En la vista `Details` se muestran los botones **aprobar** y **rechazar** .</span><span class="sxs-lookup"><span data-stu-id="1844e-275">The `Details` view shows **Approve** and **Reject** buttons.</span></span>
* <span data-ttu-id="1844e-276">Los administradores pueden aprobar o rechazar y editar o eliminar todos los datos.</span><span class="sxs-lookup"><span data-stu-id="1844e-276">Administrators can approve/reject and edit/delete all data.</span></span>

| <span data-ttu-id="1844e-277">Usuario</span><span class="sxs-lookup"><span data-stu-id="1844e-277">User</span></span>                | <span data-ttu-id="1844e-278">Inicializado por la aplicación</span><span class="sxs-lookup"><span data-stu-id="1844e-278">Seeded by the app</span></span> | <span data-ttu-id="1844e-279">Opciones</span><span class="sxs-lookup"><span data-stu-id="1844e-279">Options</span></span>                                  |
| ------------------- | :---------------: | ---------------------------------------- |
| test@example.com    | <span data-ttu-id="1844e-280">No</span><span class="sxs-lookup"><span data-stu-id="1844e-280">No</span></span>                | <span data-ttu-id="1844e-281">Edite o elimine los datos propios.</span><span class="sxs-lookup"><span data-stu-id="1844e-281">Edit/delete the own data.</span></span>                |
| manager@contoso.com | <span data-ttu-id="1844e-282">Sí</span><span class="sxs-lookup"><span data-stu-id="1844e-282">Yes</span></span>               | <span data-ttu-id="1844e-283">Aprobar/rechazar y editar o eliminar los datos propios.</span><span class="sxs-lookup"><span data-stu-id="1844e-283">Approve/reject and edit/delete own data.</span></span> |
| admin@contoso.com   | <span data-ttu-id="1844e-284">Sí</span><span class="sxs-lookup"><span data-stu-id="1844e-284">Yes</span></span>               | <span data-ttu-id="1844e-285">Aprobar/rechazar y editar o eliminar todos los datos.</span><span class="sxs-lookup"><span data-stu-id="1844e-285">Approve/reject and edit/delete all data.</span></span> |

<span data-ttu-id="1844e-286">Cree un contacto en el explorador del administrador.</span><span class="sxs-lookup"><span data-stu-id="1844e-286">Create a contact in the administrator's browser.</span></span> <span data-ttu-id="1844e-287">Copie la dirección URL para eliminar y editar del contacto del administrador.</span><span class="sxs-lookup"><span data-stu-id="1844e-287">Copy the URL for delete and edit from the administrator contact.</span></span> <span data-ttu-id="1844e-288">Pegue estos vínculos en el explorador del usuario de prueba para comprobar que el usuario de prueba no puede realizar estas operaciones.</span><span class="sxs-lookup"><span data-stu-id="1844e-288">Paste these links into the test user's browser to verify the test user can't perform these operations.</span></span>

## <a name="create-the-starter-app"></a><span data-ttu-id="1844e-289">Creación de la aplicación de inicio</span><span class="sxs-lookup"><span data-stu-id="1844e-289">Create the starter app</span></span>

* <span data-ttu-id="1844e-290">Crear una aplicación de Razor Pages denominada "ContactManager"</span><span class="sxs-lookup"><span data-stu-id="1844e-290">Create a Razor Pages app named "ContactManager"</span></span>
  * <span data-ttu-id="1844e-291">Cree la aplicación con **cuentas de usuario individuales**.</span><span class="sxs-lookup"><span data-stu-id="1844e-291">Create the app with **Individual User Accounts**.</span></span>
  * <span data-ttu-id="1844e-292">Asígnele el nombre "ContactManager" para que el espacio de nombres coincida con el espacio de nombres usado en el ejemplo.</span><span class="sxs-lookup"><span data-stu-id="1844e-292">Name it "ContactManager" so the namespace matches the namespace used in the sample.</span></span>
  * <span data-ttu-id="1844e-293">`-uld` especifica LocalDB en lugar de SQLite</span><span class="sxs-lookup"><span data-stu-id="1844e-293">`-uld` specifies LocalDB instead of SQLite</span></span>

  ```dotnetcli
  dotnet new webapp -o ContactManager -au Individual -uld
  ```

* <span data-ttu-id="1844e-294">Agregue *Models/contact. CS*:</span><span class="sxs-lookup"><span data-stu-id="1844e-294">Add *Models/Contact.cs*:</span></span>

  [!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

* <span data-ttu-id="1844e-295">Aplicar scaffolding al modelo de `Contact`.</span><span class="sxs-lookup"><span data-stu-id="1844e-295">Scaffold the `Contact` model.</span></span>
* <span data-ttu-id="1844e-296">Cree la migración inicial y actualice la base de datos:</span><span class="sxs-lookup"><span data-stu-id="1844e-296">Create initial migration and update the database:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet tool install -g dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
dotnet ef database drop -f
dotnet ef migrations add initial
dotnet ef database update
```

<span data-ttu-id="1844e-297">Si experimenta un error con el comando `dotnet aspnet-codegenerator razorpage`, consulte [este problema de github](https://github.com/aspnet/Scaffolding/issues/984).</span><span class="sxs-lookup"><span data-stu-id="1844e-297">If you experience a bug with the `dotnet aspnet-codegenerator razorpage` command, see [this GitHub issue](https://github.com/aspnet/Scaffolding/issues/984).</span></span>

* <span data-ttu-id="1844e-298">Actualice el delimitador de **ContactManager** en el archivo *pages/Shared/_Layout. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="1844e-298">Update the **ContactManager** anchor in the *Pages/Shared/_Layout.cshtml* file:</span></span>

 ```cshtml
<a class="navbar-brand" asp-area="" asp-page="/Contacts/Index">ContactManager</a>
  ```

* <span data-ttu-id="1844e-299">Prueba de la aplicación mediante la creación, edición y eliminación de un contacto</span><span class="sxs-lookup"><span data-stu-id="1844e-299">Test the app by creating, editing, and deleting a contact</span></span>

### <a name="seed-the-database"></a><span data-ttu-id="1844e-300">Inicialización de la base de datos</span><span class="sxs-lookup"><span data-stu-id="1844e-300">Seed the database</span></span>

<span data-ttu-id="1844e-301">Agregue la clase [SeedData](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter3/Data/SeedData.cs) a la carpeta de *datos* :</span><span class="sxs-lookup"><span data-stu-id="1844e-301">Add the [SeedData](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter3/Data/SeedData.cs) class to the *Data* folder:</span></span>

[!code-csharp[](secure-data/samples/starter3/Data/SeedData.cs)]

<span data-ttu-id="1844e-302">Llame a `SeedData.Initialize` desde `Main`:</span><span class="sxs-lookup"><span data-stu-id="1844e-302">Call `SeedData.Initialize` from `Main`:</span></span>

[!code-csharp[](secure-data/samples/starter3/Program.cs)]

<span data-ttu-id="1844e-303">Compruebe que la aplicación ha inicializado la base de datos.</span><span class="sxs-lookup"><span data-stu-id="1844e-303">Test that the app seeded the database.</span></span> <span data-ttu-id="1844e-304">Si hay alguna fila en la base de de contactos, el método de inicialización no se ejecuta.</span><span class="sxs-lookup"><span data-stu-id="1844e-304">If there are any rows in the contact DB, the seed method doesn't run.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1 < aspnetcore-3.0"

<span data-ttu-id="1844e-305">En este tutorial se muestra cómo crear una aplicación Web de ASP.NET Core con los datos de usuario protegidos por autorización.</span><span class="sxs-lookup"><span data-stu-id="1844e-305">This tutorial shows how to create an ASP.NET Core web app with user data protected by authorization.</span></span> <span data-ttu-id="1844e-306">Muestra una lista de contactos que los usuarios autenticados (registrados) han creado.</span><span class="sxs-lookup"><span data-stu-id="1844e-306">It displays a list of contacts that authenticated (registered) users have created.</span></span> <span data-ttu-id="1844e-307">Hay tres grupos de seguridad:</span><span class="sxs-lookup"><span data-stu-id="1844e-307">There are three security groups:</span></span>

* <span data-ttu-id="1844e-308">**Los usuarios registrados** pueden ver todos los datos aprobados y pueden editar o eliminar sus propios datos.</span><span class="sxs-lookup"><span data-stu-id="1844e-308">**Registered users** can view all the approved data and can edit/delete their own data.</span></span>
* <span data-ttu-id="1844e-309">Los **administradores** pueden aprobar o rechazar datos de contacto.</span><span class="sxs-lookup"><span data-stu-id="1844e-309">**Managers** can approve or reject contact data.</span></span> <span data-ttu-id="1844e-310">Los usuarios solo pueden ver los contactos aprobados.</span><span class="sxs-lookup"><span data-stu-id="1844e-310">Only approved contacts are visible to users.</span></span>
* <span data-ttu-id="1844e-311">**Los administradores** pueden aprobar o rechazar y editar o eliminar los datos.</span><span class="sxs-lookup"><span data-stu-id="1844e-311">**Administrators** can approve/reject and edit/delete any data.</span></span>

<span data-ttu-id="1844e-312">En la siguiente imagen, el usuario Rick (`rick@example.com`) ha iniciado sesión.</span><span class="sxs-lookup"><span data-stu-id="1844e-312">In the following image, user Rick (`rick@example.com`) is signed in.</span></span> <span data-ttu-id="1844e-313">Rick solo puede ver contactos aprobados y **editar**/**eliminar**/**crear nuevos** vínculos para sus contactos.</span><span class="sxs-lookup"><span data-stu-id="1844e-313">Rick can only view approved contacts and **Edit**/**Delete**/**Create New** links for his contacts.</span></span> <span data-ttu-id="1844e-314">Solo el último registro, creado por Rick, muestra los vínculos de **edición** y **eliminación** .</span><span class="sxs-lookup"><span data-stu-id="1844e-314">Only the last record, created by Rick, displays **Edit** and **Delete** links.</span></span> <span data-ttu-id="1844e-315">Otros usuarios no verán el último registro hasta que un administrador o un administrador cambie el estado a "aprobado".</span><span class="sxs-lookup"><span data-stu-id="1844e-315">Other users won't see the last record until a manager or administrator changes the status to "Approved".</span></span>

![Captura de pantalla que muestra Rick con sesión iniciada](secure-data/_static/rick.png)

<span data-ttu-id="1844e-317">En la imagen siguiente, `manager@contoso.com` está conectado y en el rol del administrador:</span><span class="sxs-lookup"><span data-stu-id="1844e-317">In the following image, `manager@contoso.com` is signed in and in the manager's role:</span></span>

![Captura de pantalla que muestra manager@contoso.com sesión iniciada](secure-data/_static/manager1.png)

<span data-ttu-id="1844e-319">La siguiente imagen muestra la vista de detalles de los administradores de un contacto:</span><span class="sxs-lookup"><span data-stu-id="1844e-319">The following image shows the managers details view of a contact:</span></span>

![Vista del administrador de un contacto](secure-data/_static/manager.png)

<span data-ttu-id="1844e-321">Los botones **aprobar** y **rechazar** solo se muestran para administradores y administradores.</span><span class="sxs-lookup"><span data-stu-id="1844e-321">The **Approve** and **Reject** buttons are only displayed for managers and administrators.</span></span>

<span data-ttu-id="1844e-322">En la siguiente imagen, `admin@contoso.com` está conectado y en el rol de administrador:</span><span class="sxs-lookup"><span data-stu-id="1844e-322">In the following image, `admin@contoso.com` is signed in and in the administrator's role:</span></span>

![Captura de pantalla que muestra admin@contoso.com sesión iniciada](secure-data/_static/admin.png)

<span data-ttu-id="1844e-324">El administrador tiene todos los privilegios.</span><span class="sxs-lookup"><span data-stu-id="1844e-324">The administrator has all privileges.</span></span> <span data-ttu-id="1844e-325">Puede leer, modificar o eliminar cualquier contacto y cambiar el estado de los contactos.</span><span class="sxs-lookup"><span data-stu-id="1844e-325">She can read/edit/delete any contact and change the status of contacts.</span></span>

<span data-ttu-id="1844e-326">La aplicación se creó mediante [scaffolding](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model) en el siguiente modelo de `Contact`:</span><span class="sxs-lookup"><span data-stu-id="1844e-326">The app was created by [scaffolding](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model) the following `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

<span data-ttu-id="1844e-327">El ejemplo contiene los siguientes controladores de autorización:</span><span class="sxs-lookup"><span data-stu-id="1844e-327">The sample contains the following authorization handlers:</span></span>

* <span data-ttu-id="1844e-328">`ContactIsOwnerAuthorizationHandler`: garantiza que un usuario solo pueda editar sus datos.</span><span class="sxs-lookup"><span data-stu-id="1844e-328">`ContactIsOwnerAuthorizationHandler`: Ensures that a user can only edit their data.</span></span>
* <span data-ttu-id="1844e-329">`ContactManagerAuthorizationHandler`: permite a los administradores aprobar o rechazar contactos.</span><span class="sxs-lookup"><span data-stu-id="1844e-329">`ContactManagerAuthorizationHandler`: Allows managers to approve or reject contacts.</span></span>
* <span data-ttu-id="1844e-330">`ContactAdministratorsAuthorizationHandler`: permite a los administradores aprobar o rechazar contactos y editar o eliminar contactos.</span><span class="sxs-lookup"><span data-stu-id="1844e-330">`ContactAdministratorsAuthorizationHandler`: Allows administrators to approve or reject contacts and to edit/delete contacts.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1844e-331">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="1844e-331">Prerequisites</span></span>

<span data-ttu-id="1844e-332">Este tutorial es avanzado.</span><span class="sxs-lookup"><span data-stu-id="1844e-332">This tutorial is advanced.</span></span> <span data-ttu-id="1844e-333">Debe estar familiarizado con:</span><span class="sxs-lookup"><span data-stu-id="1844e-333">You should be familiar with:</span></span>

* [<span data-ttu-id="1844e-334">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1844e-334">ASP.NET Core</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="1844e-335">Autenticación</span><span class="sxs-lookup"><span data-stu-id="1844e-335">Authentication</span></span>](xref:security/authentication/identity)
* [<span data-ttu-id="1844e-336">Confirmación de las cuentas y recuperación de contraseñas</span><span class="sxs-lookup"><span data-stu-id="1844e-336">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="1844e-337">Autorización</span><span class="sxs-lookup"><span data-stu-id="1844e-337">Authorization</span></span>](xref:security/authorization/introduction)
* [<span data-ttu-id="1844e-338">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="1844e-338">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

## <a name="the-starter-and-completed-app"></a><span data-ttu-id="1844e-339">La aplicación de inicio y completada</span><span class="sxs-lookup"><span data-stu-id="1844e-339">The starter and completed app</span></span>

<span data-ttu-id="1844e-340">[Descargue](xref:index#how-to-download-a-sample) la aplicación [completada](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples) .</span><span class="sxs-lookup"><span data-stu-id="1844e-340">[Download](xref:index#how-to-download-a-sample) the [completed](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples) app.</span></span> <span data-ttu-id="1844e-341">[Pruebe](#test-the-completed-app) la aplicación completada para familiarizarse con sus características de seguridad.</span><span class="sxs-lookup"><span data-stu-id="1844e-341">[Test](#test-the-completed-app) the completed app so you become familiar with its security features.</span></span>

### <a name="the-starter-app"></a><span data-ttu-id="1844e-342">La aplicación de inicio</span><span class="sxs-lookup"><span data-stu-id="1844e-342">The starter app</span></span>

<span data-ttu-id="1844e-343">[Descargue](xref:index#how-to-download-a-sample) la aplicación de [Inicio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/) .</span><span class="sxs-lookup"><span data-stu-id="1844e-343">[Download](xref:index#how-to-download-a-sample) the [starter](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/) app.</span></span>

<span data-ttu-id="1844e-344">Ejecute la aplicación, pulse el vínculo **ContactManager** y compruebe que puede crear, editar y eliminar un contacto.</span><span class="sxs-lookup"><span data-stu-id="1844e-344">Run the app, tap the **ContactManager** link, and verify you can create, edit, and delete a contact.</span></span>

## <a name="secure-user-data"></a><span data-ttu-id="1844e-345">Proteger los datos de usuario</span><span class="sxs-lookup"><span data-stu-id="1844e-345">Secure user data</span></span>

<span data-ttu-id="1844e-346">En las secciones siguientes se describen todos los pasos principales para crear la aplicación de datos de usuario segura.</span><span class="sxs-lookup"><span data-stu-id="1844e-346">The following sections have all the major steps to create the secure user data app.</span></span> <span data-ttu-id="1844e-347">Puede que le resulte útil consultar el proyecto completado.</span><span class="sxs-lookup"><span data-stu-id="1844e-347">You may find it helpful to refer to the completed project.</span></span>

### <a name="tie-the-contact-data-to-the-user"></a><span data-ttu-id="1844e-348">Vincular los datos de contacto al usuario</span><span class="sxs-lookup"><span data-stu-id="1844e-348">Tie the contact data to the user</span></span>

<span data-ttu-id="1844e-349">Use el identificador de usuario de [identidad](xref:security/authentication/identity) de ASP.net para asegurarse de que los usuarios puedan editar sus datos, pero no los datos de otros usuarios.</span><span class="sxs-lookup"><span data-stu-id="1844e-349">Use the ASP.NET [Identity](xref:security/authentication/identity) user ID to ensure users can edit their data, but not other users data.</span></span> <span data-ttu-id="1844e-350">Agregue `OwnerID` y `ContactStatus` al modelo de `Contact`:</span><span class="sxs-lookup"><span data-stu-id="1844e-350">Add `OwnerID` and `ContactStatus` to the `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Models/Contact.cs?name=snippet1&highlight=5-6,16-999)]

<span data-ttu-id="1844e-351">`OwnerID` es el identificador del usuario de la tabla de `AspNetUser` de la base de datos de [identidad](xref:security/authentication/identity) .</span><span class="sxs-lookup"><span data-stu-id="1844e-351">`OwnerID` is the user's ID from the `AspNetUser` table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="1844e-352">El campo `Status` determina si los usuarios generales pueden ver un contacto.</span><span class="sxs-lookup"><span data-stu-id="1844e-352">The `Status` field determines if a contact is viewable by general users.</span></span>

<span data-ttu-id="1844e-353">Cree una nueva migración y actualice la base de datos:</span><span class="sxs-lookup"><span data-stu-id="1844e-353">Create a new migration and update the database:</span></span>

```dotnetcli
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="add-role-services-to-identity"></a><span data-ttu-id="1844e-354">Agregar servicios de función a la identidad</span><span class="sxs-lookup"><span data-stu-id="1844e-354">Add Role services to Identity</span></span>

<span data-ttu-id="1844e-355">Anexe [AddRoles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) para agregar servicios de rol:</span><span class="sxs-lookup"><span data-stu-id="1844e-355">Append [AddRoles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) to add Role services:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet2&highlight=12)]

### <a name="require-authenticated-users"></a><span data-ttu-id="1844e-356">Requerir usuarios autenticados</span><span class="sxs-lookup"><span data-stu-id="1844e-356">Require authenticated users</span></span>

<span data-ttu-id="1844e-357">Establezca la Directiva de autenticación predeterminada para requerir la autenticación de los usuarios:</span><span class="sxs-lookup"><span data-stu-id="1844e-357">Set the default authentication policy to require users to be authenticated:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet&highlight=17-99)] 

 <span data-ttu-id="1844e-358">Puede rechazar la autenticación en el nivel de la página de Razor, el controlador o el método de acción con el atributo `[AllowAnonymous]`.</span><span class="sxs-lookup"><span data-stu-id="1844e-358">You can opt out of authentication at the Razor Page, controller, or action method level with the `[AllowAnonymous]` attribute.</span></span> <span data-ttu-id="1844e-359">La configuración de la Directiva de autenticación predeterminada para requerir que los usuarios se autentiquen protege los controladores y Razor Pages recién agregados.</span><span class="sxs-lookup"><span data-stu-id="1844e-359">Setting the default authentication policy to require users to be authenticated protects newly added Razor Pages and controllers.</span></span> <span data-ttu-id="1844e-360">Tener autenticación necesaria de forma predeterminada es más seguro que confiar en los nuevos controladores y Razor Pages para incluir el atributo `[Authorize]`.</span><span class="sxs-lookup"><span data-stu-id="1844e-360">Having authentication required by default is more secure than relying on new controllers and Razor Pages to include the `[Authorize]` attribute.</span></span>

<span data-ttu-id="1844e-361">Agregue [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) a las páginas index, about y Contact para que los usuarios anónimos puedan obtener información sobre el sitio antes de registrarse.</span><span class="sxs-lookup"><span data-stu-id="1844e-361">Add [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) to the Index, About, and Contact pages so anonymous users can get information about the site before they register.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Index.cshtml.cs?highlight=1,6)]

### <a name="configure-the-test-account"></a><span data-ttu-id="1844e-362">Configurar la cuenta de prueba</span><span class="sxs-lookup"><span data-stu-id="1844e-362">Configure the test account</span></span>

<span data-ttu-id="1844e-363">La clase `SeedData` crea dos cuentas: administrador y administrador.</span><span class="sxs-lookup"><span data-stu-id="1844e-363">The `SeedData` class creates two accounts: administrator and manager.</span></span> <span data-ttu-id="1844e-364">Use la [herramienta Administrador de secretos](xref:security/app-secrets) para establecer una contraseña para estas cuentas.</span><span class="sxs-lookup"><span data-stu-id="1844e-364">Use the [Secret Manager tool](xref:security/app-secrets) to set a password for these accounts.</span></span> <span data-ttu-id="1844e-365">Establezca la contraseña desde el directorio del proyecto (el directorio que contiene *Program.CS*):</span><span class="sxs-lookup"><span data-stu-id="1844e-365">Set the password from the project directory (the directory containing *Program.cs*):</span></span>

```dotnetcli
dotnet user-secrets set SeedUserPW <PW>
```

<span data-ttu-id="1844e-366">Si no se especifica una contraseña segura, se produce una excepción cuando se llama a `SeedData.Initialize`.</span><span class="sxs-lookup"><span data-stu-id="1844e-366">If a strong password is not specified, an exception is thrown when `SeedData.Initialize` is called.</span></span>

<span data-ttu-id="1844e-367">Actualice `Main` para usar la contraseña de prueba:</span><span class="sxs-lookup"><span data-stu-id="1844e-367">Update `Main` to use the test password:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a><span data-ttu-id="1844e-368">Crear las cuentas de prueba y actualizar los contactos</span><span class="sxs-lookup"><span data-stu-id="1844e-368">Create the test accounts and update the contacts</span></span>

<span data-ttu-id="1844e-369">Actualice el método `Initialize` de la clase `SeedData` para crear las cuentas de prueba:</span><span class="sxs-lookup"><span data-stu-id="1844e-369">Update the `Initialize` method in the `SeedData` class to create the test accounts:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Data/SeedData.cs?name=snippet_Initialize)]

<span data-ttu-id="1844e-370">Agregue el identificador de usuario de administrador y el `ContactStatus` a los contactos.</span><span class="sxs-lookup"><span data-stu-id="1844e-370">Add the administrator user ID and `ContactStatus` to the contacts.</span></span> <span data-ttu-id="1844e-371">Haga que uno de los contactos "enviado" y otro "rechazado".</span><span class="sxs-lookup"><span data-stu-id="1844e-371">Make one of the contacts "Submitted" and one "Rejected".</span></span> <span data-ttu-id="1844e-372">Agregue el identificador de usuario y el estado a todos los contactos.</span><span class="sxs-lookup"><span data-stu-id="1844e-372">Add the user ID and status to all the contacts.</span></span> <span data-ttu-id="1844e-373">Solo se muestra un contacto:</span><span class="sxs-lookup"><span data-stu-id="1844e-373">Only one contact is shown:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a><span data-ttu-id="1844e-374">Crear controladores de autorización de propietario, administrador y administrador</span><span class="sxs-lookup"><span data-stu-id="1844e-374">Create owner, manager, and administrator authorization handlers</span></span>

<span data-ttu-id="1844e-375">Cree una carpeta de *autorización* y cree una `ContactIsOwnerAuthorizationHandler` clase en ella.</span><span class="sxs-lookup"><span data-stu-id="1844e-375">Create an *Authorization* folder and create a `ContactIsOwnerAuthorizationHandler` class in it.</span></span> <span data-ttu-id="1844e-376">En el `ContactIsOwnerAuthorizationHandler` se comprueba que el usuario que actúa en un recurso posee el recurso.</span><span class="sxs-lookup"><span data-stu-id="1844e-376">The `ContactIsOwnerAuthorizationHandler` verifies that the user acting on a resource owns the resource.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

<span data-ttu-id="1844e-377">El `ContactIsOwnerAuthorizationHandler` llama al [contexto. Se realiza correctamente](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) si el usuario autenticado actual es el propietario del contacto.</span><span class="sxs-lookup"><span data-stu-id="1844e-377">The `ContactIsOwnerAuthorizationHandler` calls [context.Succeed](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) if the current authenticated user is the contact owner.</span></span> <span data-ttu-id="1844e-378">Los controladores de autorización generalmente:</span><span class="sxs-lookup"><span data-stu-id="1844e-378">Authorization handlers generally:</span></span>

* <span data-ttu-id="1844e-379">Devuelve `context.Succeed` cuando se cumplen los requisitos.</span><span class="sxs-lookup"><span data-stu-id="1844e-379">Return `context.Succeed` when the requirements are met.</span></span>
* <span data-ttu-id="1844e-380">Devuelve `Task.CompletedTask` cuando no se cumplen los requisitos.</span><span class="sxs-lookup"><span data-stu-id="1844e-380">Return `Task.CompletedTask` when requirements aren't met.</span></span> <span data-ttu-id="1844e-381">`Task.CompletedTask` no es correcto o error&mdash;permite que se ejecuten otros controladores de autorización.</span><span class="sxs-lookup"><span data-stu-id="1844e-381">`Task.CompletedTask` is not success or failure&mdash;it allows other authorization handlers to run.</span></span>

<span data-ttu-id="1844e-382">Si necesita generar un error explícitamente, devuelva el [contexto. Error](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span><span class="sxs-lookup"><span data-stu-id="1844e-382">If you need to explicitly fail, return [context.Fail](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span></span>

<span data-ttu-id="1844e-383">La aplicación permite que los propietarios de contactos editen o eliminen o creen sus propios datos.</span><span class="sxs-lookup"><span data-stu-id="1844e-383">The app allows contact owners to edit/delete/create their own data.</span></span> <span data-ttu-id="1844e-384">`ContactIsOwnerAuthorizationHandler` no necesita comprobar la operación que se ha pasado en el parámetro de requisito.</span><span class="sxs-lookup"><span data-stu-id="1844e-384">`ContactIsOwnerAuthorizationHandler` doesn't need to check the operation passed in the requirement parameter.</span></span>

### <a name="create-a-manager-authorization-handler"></a><span data-ttu-id="1844e-385">Crear un controlador de autorización de administrador</span><span class="sxs-lookup"><span data-stu-id="1844e-385">Create a manager authorization handler</span></span>

<span data-ttu-id="1844e-386">Cree una clase de `ContactManagerAuthorizationHandler` en la carpeta de *autorización* .</span><span class="sxs-lookup"><span data-stu-id="1844e-386">Create a `ContactManagerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="1844e-387">El `ContactManagerAuthorizationHandler` comprueba que el usuario que actúa en el recurso es un administrador.</span><span class="sxs-lookup"><span data-stu-id="1844e-387">The `ContactManagerAuthorizationHandler` verifies the user acting on the resource is a manager.</span></span> <span data-ttu-id="1844e-388">Solo los administradores pueden aprobar o rechazar cambios de contenido (nuevos o modificados).</span><span class="sxs-lookup"><span data-stu-id="1844e-388">Only managers can approve or reject content changes (new or changed).</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a><span data-ttu-id="1844e-389">Crear un controlador de autorización de administrador</span><span class="sxs-lookup"><span data-stu-id="1844e-389">Create an administrator authorization handler</span></span>

<span data-ttu-id="1844e-390">Cree una clase de `ContactAdministratorsAuthorizationHandler` en la carpeta de *autorización* .</span><span class="sxs-lookup"><span data-stu-id="1844e-390">Create a `ContactAdministratorsAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="1844e-391">El `ContactAdministratorsAuthorizationHandler` comprueba que el usuario que actúa en el recurso es un administrador.</span><span class="sxs-lookup"><span data-stu-id="1844e-391">The `ContactAdministratorsAuthorizationHandler` verifies the user acting on the resource is an administrator.</span></span> <span data-ttu-id="1844e-392">El administrador puede realizar todas las operaciones.</span><span class="sxs-lookup"><span data-stu-id="1844e-392">Administrator can do all operations.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a><span data-ttu-id="1844e-393">Registrar los controladores de autorización</span><span class="sxs-lookup"><span data-stu-id="1844e-393">Register the authorization handlers</span></span>

<span data-ttu-id="1844e-394">Los servicios que usan Entity Framework Core deben estar registrados para la [inserción de dependencias](xref:fundamentals/dependency-injection) mediante [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span><span class="sxs-lookup"><span data-stu-id="1844e-394">Services using Entity Framework Core must be registered for [dependency injection](xref:fundamentals/dependency-injection) using [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span></span> <span data-ttu-id="1844e-395">En el `ContactIsOwnerAuthorizationHandler` se usa ASP.NET Core [Identity](xref:security/authentication/identity), que se basa en Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="1844e-395">The `ContactIsOwnerAuthorizationHandler` uses ASP.NET Core [Identity](xref:security/authentication/identity), which is built on Entity Framework Core.</span></span> <span data-ttu-id="1844e-396">Registre los controladores con la colección de servicios para que estén disponibles para el `ContactsController` a través de la [inserción de dependencias](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="1844e-396">Register the handlers with the service collection so they're available to the `ContactsController` through [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="1844e-397">Agregue el código siguiente al final de `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="1844e-397">Add the following code to the end of `ConfigureServices`:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet_defaultPolicy&highlight=27-99)]

<span data-ttu-id="1844e-398">`ContactAdministratorsAuthorizationHandler` y `ContactManagerAuthorizationHandler` se agregan como singletons.</span><span class="sxs-lookup"><span data-stu-id="1844e-398">`ContactAdministratorsAuthorizationHandler` and `ContactManagerAuthorizationHandler` are added as singletons.</span></span> <span data-ttu-id="1844e-399">Son singleton porque no usan EF y toda la información necesaria está en el parámetro `Context` del método `HandleRequirementAsync`.</span><span class="sxs-lookup"><span data-stu-id="1844e-399">They're singletons because they don't use EF and all the information needed is in the `Context` parameter of the `HandleRequirementAsync` method.</span></span>

## <a name="support-authorization"></a><span data-ttu-id="1844e-400">Autorización de soporte técnico</span><span class="sxs-lookup"><span data-stu-id="1844e-400">Support authorization</span></span>

<span data-ttu-id="1844e-401">En esta sección, actualizará el Razor Pages y agregará una clase de requisitos de operaciones.</span><span class="sxs-lookup"><span data-stu-id="1844e-401">In this section, you update the Razor Pages and add an operations requirements class.</span></span>

### <a name="review-the-contact-operations-requirements-class"></a><span data-ttu-id="1844e-402">Revisar la clase de requisitos de las operaciones de contacto</span><span class="sxs-lookup"><span data-stu-id="1844e-402">Review the contact operations requirements class</span></span>

<span data-ttu-id="1844e-403">Revise la clase `ContactOperations`.</span><span class="sxs-lookup"><span data-stu-id="1844e-403">Review the `ContactOperations` class.</span></span> <span data-ttu-id="1844e-404">Esta clase contiene los requisitos que admite la aplicación:</span><span class="sxs-lookup"><span data-stu-id="1844e-404">This class contains the requirements the app supports:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-contacts-razor-pages"></a><span data-ttu-id="1844e-405">Cree una clase base para el Razor Pages contactos</span><span class="sxs-lookup"><span data-stu-id="1844e-405">Create a base class for the Contacts Razor Pages</span></span>

<span data-ttu-id="1844e-406">Cree una clase base que contenga los servicios usados en el Razor Pages contactos.</span><span class="sxs-lookup"><span data-stu-id="1844e-406">Create a base class that contains the services used in the contacts Razor Pages.</span></span> <span data-ttu-id="1844e-407">La clase base coloca el código de inicialización en una ubicación:</span><span class="sxs-lookup"><span data-stu-id="1844e-407">The base class puts the initialization code in one location:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/DI_BasePageModel.cs)]

<span data-ttu-id="1844e-408">El código anterior:</span><span class="sxs-lookup"><span data-stu-id="1844e-408">The preceding code:</span></span>

* <span data-ttu-id="1844e-409">Agrega el servicio `IAuthorizationService` para tener acceso a los controladores de autorización.</span><span class="sxs-lookup"><span data-stu-id="1844e-409">Adds the `IAuthorizationService` service to access to the authorization handlers.</span></span>
* <span data-ttu-id="1844e-410">Agrega el servicio Identity `UserManager`.</span><span class="sxs-lookup"><span data-stu-id="1844e-410">Adds the Identity `UserManager` service.</span></span>
* <span data-ttu-id="1844e-411">Agregue la `ApplicationDbContext`.</span><span class="sxs-lookup"><span data-stu-id="1844e-411">Add the `ApplicationDbContext`.</span></span>

### <a name="update-the-createmodel"></a><span data-ttu-id="1844e-412">Actualización de CreateModel</span><span class="sxs-lookup"><span data-stu-id="1844e-412">Update the CreateModel</span></span>

<span data-ttu-id="1844e-413">Actualice el constructor Create Page Model para usar la clase base `DI_BasePageModel`:</span><span class="sxs-lookup"><span data-stu-id="1844e-413">Update the create page model constructor to use the `DI_BasePageModel` base class:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

<span data-ttu-id="1844e-414">Actualice el método `CreateModel.OnPostAsync` para:</span><span class="sxs-lookup"><span data-stu-id="1844e-414">Update the `CreateModel.OnPostAsync` method to:</span></span>

* <span data-ttu-id="1844e-415">Agregue el identificador de usuario al modelo de `Contact`.</span><span class="sxs-lookup"><span data-stu-id="1844e-415">Add the user ID to the `Contact` model.</span></span>
* <span data-ttu-id="1844e-416">Llame al controlador de autorización para comprobar que el usuario tiene permiso para crear contactos.</span><span class="sxs-lookup"><span data-stu-id="1844e-416">Call the authorization handler to verify the user has permission to create contacts.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a><span data-ttu-id="1844e-417">Actualización de IndexModel</span><span class="sxs-lookup"><span data-stu-id="1844e-417">Update the IndexModel</span></span>

<span data-ttu-id="1844e-418">Actualice el método de `OnGetAsync` de modo que solo se muestren los contactos aprobados a los usuarios generales:</span><span class="sxs-lookup"><span data-stu-id="1844e-418">Update the `OnGetAsync` method so only approved contacts are shown to general users:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a><span data-ttu-id="1844e-419">Actualización de EditModel</span><span class="sxs-lookup"><span data-stu-id="1844e-419">Update the EditModel</span></span>

<span data-ttu-id="1844e-420">Agregue un controlador de autorización para comprobar que el usuario es el propietario del contacto.</span><span class="sxs-lookup"><span data-stu-id="1844e-420">Add an authorization handler to verify the user owns the contact.</span></span> <span data-ttu-id="1844e-421">Dado que se está validando la autorización de recursos, el atributo `[Authorize]` no es suficiente.</span><span class="sxs-lookup"><span data-stu-id="1844e-421">Because resource authorization is being validated, the `[Authorize]` attribute is not enough.</span></span> <span data-ttu-id="1844e-422">La aplicación no tiene acceso al recurso cuando se evalúan los atributos.</span><span class="sxs-lookup"><span data-stu-id="1844e-422">The app doesn't have access to the resource when attributes are evaluated.</span></span> <span data-ttu-id="1844e-423">La autorización basada en recursos debe ser imperativa.</span><span class="sxs-lookup"><span data-stu-id="1844e-423">Resource-based authorization must be imperative.</span></span> <span data-ttu-id="1844e-424">Las comprobaciones deben realizarse una vez que la aplicación tenga acceso al recurso, ya sea mediante su carga en el modelo de página o mediante su carga en el propio controlador.</span><span class="sxs-lookup"><span data-stu-id="1844e-424">Checks must be performed once the app has access to the resource, either by loading it in the page model or by loading it within the handler itself.</span></span> <span data-ttu-id="1844e-425">Frecuentemente tiene acceso al recurso pasando la clave de recurso.</span><span class="sxs-lookup"><span data-stu-id="1844e-425">You frequently access the resource by passing in the resource key.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a><span data-ttu-id="1844e-426">Actualización de DeleteModel</span><span class="sxs-lookup"><span data-stu-id="1844e-426">Update the DeleteModel</span></span>

<span data-ttu-id="1844e-427">Actualice el modelo de página de eliminación para que use el controlador de autorización para comprobar que el usuario tiene el permiso eliminar en el contacto.</span><span class="sxs-lookup"><span data-stu-id="1844e-427">Update the delete page model to use the authorization handler to verify the user has delete permission on the contact.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a><span data-ttu-id="1844e-428">Inyectar el servicio de autorización en las vistas</span><span class="sxs-lookup"><span data-stu-id="1844e-428">Inject the authorization service into the views</span></span>

<span data-ttu-id="1844e-429">Actualmente, la interfaz de usuario muestra vínculos de edición y eliminación para contactos que el usuario no puede modificar.</span><span class="sxs-lookup"><span data-stu-id="1844e-429">Currently, the UI shows edit and delete links for contacts the user can't modify.</span></span>

<span data-ttu-id="1844e-430">Inserte el servicio de autorización en el archivo *views/_ViewImports. cshtml* para que esté disponible para todas las vistas:</span><span class="sxs-lookup"><span data-stu-id="1844e-430">Inject the authorization service in the *Views/_ViewImports.cshtml* file so it's available to all views:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/_ViewImports.cshtml?highlight=6-99)]

<span data-ttu-id="1844e-431">El marcado anterior agrega varias instrucciones de `using`.</span><span class="sxs-lookup"><span data-stu-id="1844e-431">The preceding markup adds several `using` statements.</span></span>

<span data-ttu-id="1844e-432">Actualice los vínculos de **edición** y **eliminación** en *pages/Contacts/index. cshtml* para que solo se representen para los usuarios con los permisos adecuados:</span><span class="sxs-lookup"><span data-stu-id="1844e-432">Update the **Edit** and **Delete** links in *Pages/Contacts/Index.cshtml* so they're only rendered for users with the appropriate permissions:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Index.cshtml?highlight=34-36,62-999)]

> [!WARNING]
> <span data-ttu-id="1844e-433">Ocultar los vínculos de los usuarios que no tienen permiso para cambiar datos no protege la aplicación.</span><span class="sxs-lookup"><span data-stu-id="1844e-433">Hiding links from users that don't have permission to change data doesn't secure the app.</span></span> <span data-ttu-id="1844e-434">La ocultación de los vínculos hace que la aplicación sea más fácil de ver mostrando solo vínculos válidos.</span><span class="sxs-lookup"><span data-stu-id="1844e-434">Hiding links makes the app more user-friendly by displaying only valid links.</span></span> <span data-ttu-id="1844e-435">Los usuarios pueden atacar las direcciones URL generadas para invocar operaciones de edición y eliminación en los datos que no son de su propiedad.</span><span class="sxs-lookup"><span data-stu-id="1844e-435">Users can hack the generated URLs to invoke edit and delete operations on data they don't own.</span></span> <span data-ttu-id="1844e-436">La página o el controlador de Razor deben aplicar las comprobaciones de acceso para proteger los datos.</span><span class="sxs-lookup"><span data-stu-id="1844e-436">The Razor Page or controller must enforce access checks to secure the data.</span></span>

### <a name="update-details"></a><span data-ttu-id="1844e-437">Detalles de la actualización</span><span class="sxs-lookup"><span data-stu-id="1844e-437">Update Details</span></span>

<span data-ttu-id="1844e-438">Actualice la vista de detalles para que los administradores puedan aprobar o rechazar contactos:</span><span class="sxs-lookup"><span data-stu-id="1844e-438">Update the details view so managers can approve or reject contacts:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml?name=snippet)]

<span data-ttu-id="1844e-439">Actualice el modelo de página de detalles:</span><span class="sxs-lookup"><span data-stu-id="1844e-439">Update the details page model:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="add-or-remove-a-user-to-a-role"></a><span data-ttu-id="1844e-440">Agregar o quitar un usuario de un rol</span><span class="sxs-lookup"><span data-stu-id="1844e-440">Add or remove a user to a role</span></span>

<span data-ttu-id="1844e-441">Consulte [este problema](https://github.com/aspnet/AspNetCore.Docs/issues/8502) para obtener información sobre:</span><span class="sxs-lookup"><span data-stu-id="1844e-441">See [this issue](https://github.com/aspnet/AspNetCore.Docs/issues/8502) for information on:</span></span>

* <span data-ttu-id="1844e-442">Quitar privilegios de un usuario.</span><span class="sxs-lookup"><span data-stu-id="1844e-442">Removing privileges from a user.</span></span> <span data-ttu-id="1844e-443">Por ejemplo, silenciar a un usuario en una aplicación de chat.</span><span class="sxs-lookup"><span data-stu-id="1844e-443">For example, muting a user in a chat app.</span></span>
* <span data-ttu-id="1844e-444">Agregar privilegios a un usuario.</span><span class="sxs-lookup"><span data-stu-id="1844e-444">Adding privileges to a user.</span></span>

## <a name="test-the-completed-app"></a><span data-ttu-id="1844e-445">Prueba de la aplicación completada</span><span class="sxs-lookup"><span data-stu-id="1844e-445">Test the completed app</span></span>

<span data-ttu-id="1844e-446">Si aún no ha establecido una contraseña para las cuentas de usuario inicializadas, use la [herramienta Administrador de secretos](xref:security/app-secrets#secret-manager) para establecer una contraseña:</span><span class="sxs-lookup"><span data-stu-id="1844e-446">If you haven't already set a password for seeded user accounts, use the [Secret Manager tool](xref:security/app-secrets#secret-manager) to set a password:</span></span>

* <span data-ttu-id="1844e-447">Elija una contraseña segura: Use ocho o más caracteres y, al menos, un carácter en mayúscula, un número y un símbolo.</span><span class="sxs-lookup"><span data-stu-id="1844e-447">Choose a strong password: Use eight or more characters and at least one upper-case character, number, and symbol.</span></span> <span data-ttu-id="1844e-448">Por ejemplo, `Passw0rd!` cumple los requisitos de contraseñas seguras.</span><span class="sxs-lookup"><span data-stu-id="1844e-448">For example, `Passw0rd!` meets the strong password requirements.</span></span>
* <span data-ttu-id="1844e-449">Ejecute el siguiente comando desde la carpeta del proyecto, donde `<PW>` es la contraseña:</span><span class="sxs-lookup"><span data-stu-id="1844e-449">Execute the following command from the project's folder, where `<PW>` is the password:</span></span>

  ```dotnetcli
  dotnet user-secrets set SeedUserPW <PW>
  ```

* <span data-ttu-id="1844e-450">Quitar y actualizar la base de datos</span><span class="sxs-lookup"><span data-stu-id="1844e-450">Drop and update the Database</span></span>

  ```dotnetcli
  dotnet ef database drop -f
  dotnet ef database update  
  ```

* <span data-ttu-id="1844e-451">Reinicie la aplicación para inicializar la base de datos.</span><span class="sxs-lookup"><span data-stu-id="1844e-451">Restart the app to seed the database.</span></span>

<span data-ttu-id="1844e-452">Una manera fácil de probar la aplicación completada es iniciar tres exploradores diferentes (o sesiones de incógnito/InPrivate).</span><span class="sxs-lookup"><span data-stu-id="1844e-452">An easy way to test the completed app is to launch three different browsers (or incognito/InPrivate sessions).</span></span> <span data-ttu-id="1844e-453">En un explorador, registre un nuevo usuario (por ejemplo, `test@example.com`).</span><span class="sxs-lookup"><span data-stu-id="1844e-453">In one browser, register a new user (for example, `test@example.com`).</span></span> <span data-ttu-id="1844e-454">Inicie sesión en cada explorador con un usuario diferente.</span><span class="sxs-lookup"><span data-stu-id="1844e-454">Sign in to each browser with a different user.</span></span> <span data-ttu-id="1844e-455">Compruebe las siguientes operaciones:</span><span class="sxs-lookup"><span data-stu-id="1844e-455">Verify the following operations:</span></span>

* <span data-ttu-id="1844e-456">Los usuarios registrados pueden ver todos los datos de contacto aprobados.</span><span class="sxs-lookup"><span data-stu-id="1844e-456">Registered users can view all of the approved contact data.</span></span>
* <span data-ttu-id="1844e-457">Los usuarios registrados pueden editar o eliminar sus propios datos.</span><span class="sxs-lookup"><span data-stu-id="1844e-457">Registered users can edit/delete their own data.</span></span>
* <span data-ttu-id="1844e-458">Los administradores pueden aprobar o rechazar datos de contacto.</span><span class="sxs-lookup"><span data-stu-id="1844e-458">Managers can approve/reject contact data.</span></span> <span data-ttu-id="1844e-459">En la vista `Details` se muestran los botones **aprobar** y **rechazar** .</span><span class="sxs-lookup"><span data-stu-id="1844e-459">The `Details` view shows **Approve** and **Reject** buttons.</span></span>
* <span data-ttu-id="1844e-460">Los administradores pueden aprobar o rechazar y editar o eliminar todos los datos.</span><span class="sxs-lookup"><span data-stu-id="1844e-460">Administrators can approve/reject and edit/delete all data.</span></span>

| <span data-ttu-id="1844e-461">Usuario</span><span class="sxs-lookup"><span data-stu-id="1844e-461">User</span></span>                | <span data-ttu-id="1844e-462">Inicializado por la aplicación</span><span class="sxs-lookup"><span data-stu-id="1844e-462">Seeded by the app</span></span> | <span data-ttu-id="1844e-463">Opciones</span><span class="sxs-lookup"><span data-stu-id="1844e-463">Options</span></span>                                  |
| ------------------- | :---------------: | ---------------------------------------- |
| test@example.com    | <span data-ttu-id="1844e-464">No</span><span class="sxs-lookup"><span data-stu-id="1844e-464">No</span></span>                | <span data-ttu-id="1844e-465">Edite o elimine los datos propios.</span><span class="sxs-lookup"><span data-stu-id="1844e-465">Edit/delete the own data.</span></span>                |
| manager@contoso.com | <span data-ttu-id="1844e-466">Sí</span><span class="sxs-lookup"><span data-stu-id="1844e-466">Yes</span></span>               | <span data-ttu-id="1844e-467">Aprobar/rechazar y editar o eliminar los datos propios.</span><span class="sxs-lookup"><span data-stu-id="1844e-467">Approve/reject and edit/delete own data.</span></span> |
| admin@contoso.com   | <span data-ttu-id="1844e-468">Sí</span><span class="sxs-lookup"><span data-stu-id="1844e-468">Yes</span></span>               | <span data-ttu-id="1844e-469">Aprobar/rechazar y editar o eliminar todos los datos.</span><span class="sxs-lookup"><span data-stu-id="1844e-469">Approve/reject and edit/delete all data.</span></span> |

<span data-ttu-id="1844e-470">Cree un contacto en el explorador del administrador.</span><span class="sxs-lookup"><span data-stu-id="1844e-470">Create a contact in the administrator's browser.</span></span> <span data-ttu-id="1844e-471">Copie la dirección URL para eliminar y editar del contacto del administrador.</span><span class="sxs-lookup"><span data-stu-id="1844e-471">Copy the URL for delete and edit from the administrator contact.</span></span> <span data-ttu-id="1844e-472">Pegue estos vínculos en el explorador del usuario de prueba para comprobar que el usuario de prueba no puede realizar estas operaciones.</span><span class="sxs-lookup"><span data-stu-id="1844e-472">Paste these links into the test user's browser to verify the test user can't perform these operations.</span></span>

## <a name="create-the-starter-app"></a><span data-ttu-id="1844e-473">Creación de la aplicación de inicio</span><span class="sxs-lookup"><span data-stu-id="1844e-473">Create the starter app</span></span>

* <span data-ttu-id="1844e-474">Crear una aplicación de Razor Pages denominada "ContactManager"</span><span class="sxs-lookup"><span data-stu-id="1844e-474">Create a Razor Pages app named "ContactManager"</span></span>
  * <span data-ttu-id="1844e-475">Cree la aplicación con **cuentas de usuario individuales**.</span><span class="sxs-lookup"><span data-stu-id="1844e-475">Create the app with **Individual User Accounts**.</span></span>
  * <span data-ttu-id="1844e-476">Asígnele el nombre "ContactManager" para que el espacio de nombres coincida con el espacio de nombres usado en el ejemplo.</span><span class="sxs-lookup"><span data-stu-id="1844e-476">Name it "ContactManager" so the namespace matches the namespace used in the sample.</span></span>
  * <span data-ttu-id="1844e-477">`-uld` especifica LocalDB en lugar de SQLite</span><span class="sxs-lookup"><span data-stu-id="1844e-477">`-uld` specifies LocalDB instead of SQLite</span></span>

  ```dotnetcli
  dotnet new webapp -o ContactManager -au Individual -uld
  ```

* <span data-ttu-id="1844e-478">Agregue *Models/contact. CS*:</span><span class="sxs-lookup"><span data-stu-id="1844e-478">Add *Models/Contact.cs*:</span></span>

  [!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

* <span data-ttu-id="1844e-479">Aplicar scaffolding al modelo de `Contact`.</span><span class="sxs-lookup"><span data-stu-id="1844e-479">Scaffold the `Contact` model.</span></span>
* <span data-ttu-id="1844e-480">Cree la migración inicial y actualice la base de datos:</span><span class="sxs-lookup"><span data-stu-id="1844e-480">Create initial migration and update the database:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
  dotnet ef database drop -f
  dotnet ef migrations add initial
  dotnet ef database update
  ```

* <span data-ttu-id="1844e-481">Actualice el delimitador de **ContactManager** en el archivo *pages/_Layout. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="1844e-481">Update the **ContactManager** anchor in the *Pages/_Layout.cshtml* file:</span></span>

  ```cshtml
  <a asp-page="/Contacts/Index" class="navbar-brand">ContactManager</a>
  ```

* <span data-ttu-id="1844e-482">Prueba de la aplicación mediante la creación, edición y eliminación de un contacto</span><span class="sxs-lookup"><span data-stu-id="1844e-482">Test the app by creating, editing, and deleting a contact</span></span>

### <a name="seed-the-database"></a><span data-ttu-id="1844e-483">Inicialización de la base de datos</span><span class="sxs-lookup"><span data-stu-id="1844e-483">Seed the database</span></span>

<span data-ttu-id="1844e-484">Agregue la clase [SeedData](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2.1/Data/SeedData.cs) a la carpeta de *datos* .</span><span class="sxs-lookup"><span data-stu-id="1844e-484">Add the [SeedData](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2.1/Data/SeedData.cs) class to the *Data* folder.</span></span>

<span data-ttu-id="1844e-485">Llame a `SeedData.Initialize` desde `Main`:</span><span class="sxs-lookup"><span data-stu-id="1844e-485">Call `SeedData.Initialize` from `Main`:</span></span>

[!code-csharp[](secure-data/samples/starter2.1/Program.cs?name=snippet)]

<span data-ttu-id="1844e-486">Compruebe que la aplicación ha inicializado la base de datos.</span><span class="sxs-lookup"><span data-stu-id="1844e-486">Test that the app seeded the database.</span></span> <span data-ttu-id="1844e-487">Si hay alguna fila en la base de de contactos, el método de inicialización no se ejecuta.</span><span class="sxs-lookup"><span data-stu-id="1844e-487">If there are any rows in the contact DB, the seed method doesn't run.</span></span>

::: moniker-end

<a name="secure-data-add-resources-label"></a>

### <a name="additional-resources"></a><span data-ttu-id="1844e-488">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="1844e-488">Additional resources</span></span>

* [<span data-ttu-id="1844e-489">Compilación de una aplicación Web de .NET Core y SQL Database en Azure App Service</span><span class="sxs-lookup"><span data-stu-id="1844e-489">Build a .NET Core and SQL Database web app in Azure App Service</span></span>](/azure/app-service/app-service-web-tutorial-dotnetcore-sqldb)
* <span data-ttu-id="1844e-490">[ASP.net Core laboratorio de autorización](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span><span class="sxs-lookup"><span data-stu-id="1844e-490">[ASP.NET Core Authorization Lab](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span></span> <span data-ttu-id="1844e-491">En este laboratorio se incluyen más detalles sobre las características de seguridad que se presentan en este tutorial.</span><span class="sxs-lookup"><span data-stu-id="1844e-491">This lab goes into more detail on the security features introduced in this tutorial.</span></span>
* <xref:security/authorization/introduction>
* [<span data-ttu-id="1844e-492">Autorización personalizada basada en directivas</span><span class="sxs-lookup"><span data-stu-id="1844e-492">Custom policy-based authorization</span></span>](xref:security/authorization/policies)
