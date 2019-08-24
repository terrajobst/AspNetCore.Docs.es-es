---
title: Crear una aplicación ASP.NET Core con datos de usuario protegidos por autorización
author: rick-anderson
description: Obtenga información sobre cómo crear una aplicación de páginas de Razor con datos de usuario protegidos por autorización. Incluye HTTPS, autenticación, seguridad, ASP.NET Core Identity.
ms.author: riande
ms.date: 12/18/2018
ms.custom: mvc, seodec18
uid: security/authorization/secure-data
ms.openlocfilehash: 225d0e3aa51745253d03e614b1c8568b3a6ba731
ms.sourcegitcommit: 983b31449fe398e6e922eb13e9eb6f4287ec91e8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/24/2019
ms.locfileid: "70017492"
---
# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a><span data-ttu-id="2d9b7-104">Crear una aplicación ASP.NET Core con datos de usuario protegidos por autorización</span><span class="sxs-lookup"><span data-stu-id="2d9b7-104">Create an ASP.NET Core app with user data protected by authorization</span></span>

<span data-ttu-id="2d9b7-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT) y [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="2d9b7-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="2d9b7-106">Consulte [este PDF](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) para la versión de ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-106">See [this PDF](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core MVC version.</span></span> <span data-ttu-id="2d9b7-107">La versión 1.1 de ASP.NET Core de este tutorial se encuentra en [esto](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data) carpeta.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-107">The ASP.NET Core 1.1 version of this tutorial is in [this](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data) folder.</span></span> <span data-ttu-id="2d9b7-108">La 1.1 de ejemplo de ASP.NET Core se encuentra en la [ejemplos](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).</span><span class="sxs-lookup"><span data-stu-id="2d9b7-108">The 1.1 ASP.NET Core sample is in the [samples](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="2d9b7-109">Consulte [este pdf](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_July16_18.pdf)</span><span class="sxs-lookup"><span data-stu-id="2d9b7-109">See [this pdf](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_July16_18.pdf)</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="2d9b7-110">Este tutorial muestra cómo crear una aplicación web ASP.NET Core con los datos protegidos por autorización del usuario.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-110">This tutorial shows how to create an ASP.NET Core web app with user data protected by authorization.</span></span> <span data-ttu-id="2d9b7-111">Muestra una lista de contactos que autentican los usuarios (registrados) ha creado.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-111">It displays a list of contacts that authenticated (registered) users have created.</span></span> <span data-ttu-id="2d9b7-112">Hay tres grupos de seguridad:</span><span class="sxs-lookup"><span data-stu-id="2d9b7-112">There are three security groups:</span></span>

* <span data-ttu-id="2d9b7-113">**Usuarios registrados** puede ver todos los datos aprobados y pueden sus propios datos de editar o eliminar.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-113">**Registered users** can view all the approved data and can edit/delete their own data.</span></span>
* <span data-ttu-id="2d9b7-114">**Los administradores de** puede aprobar o rechazar los datos de contacto.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-114">**Managers** can approve or reject contact data.</span></span> <span data-ttu-id="2d9b7-115">Solo contactos aprobados son visibles para los usuarios.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-115">Only approved contacts are visible to users.</span></span>
* <span data-ttu-id="2d9b7-116">**Los administradores** puede aprobar o rechazar y editar o eliminar todos los datos.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-116">**Administrators** can approve/reject and edit/delete any data.</span></span>

<span data-ttu-id="2d9b7-117">Las imágenes de este documento no coinciden exactamente con las plantillas más recientes.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-117">The images in this document don't exactly match the latest templates.</span></span>

<span data-ttu-id="2d9b7-118">En la siguiente imagen, el usuario Rick (`rick@example.com`) ha iniciado sesión.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-118">In the following image, user Rick (`rick@example.com`) is signed in.</span></span> <span data-ttu-id="2d9b7-119">Rick solo puede ver los contactos aprobados y **editar**/**eliminar**/**crear nuevo** vínculos para sus contactos.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-119">Rick can only view approved contacts and **Edit**/**Delete**/**Create New** links for his contacts.</span></span> <span data-ttu-id="2d9b7-120">El último registro, creado por Rick, muestra **editar** y **eliminar** vínculos.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-120">Only the last record, created by Rick, displays **Edit** and **Delete** links.</span></span> <span data-ttu-id="2d9b7-121">Otros usuarios no verán el último registro hasta un administrador o un administrador cambia el estado a "Aprobado".</span><span class="sxs-lookup"><span data-stu-id="2d9b7-121">Other users won't see the last record until a manager or administrator changes the status to "Approved".</span></span>

![Captura de pantalla con Rick ha iniciado sesión](secure-data/_static/rick.png)

<span data-ttu-id="2d9b7-123">En la siguiente imagen, `manager@contoso.com` ha iniciado sesión y en el rol del administrador:</span><span class="sxs-lookup"><span data-stu-id="2d9b7-123">In the following image, `manager@contoso.com` is signed in and in the manager's role:</span></span>

![Captura de pantalla mostrando manager@contoso.com iniciada](secure-data/_static/manager1.png)

<span data-ttu-id="2d9b7-125">La siguiente imagen muestra a los administradores de la vista de detalles de un contacto:</span><span class="sxs-lookup"><span data-stu-id="2d9b7-125">The following image shows the managers details view of a contact:</span></span>

![Vista del Administrador de un contacto](secure-data/_static/manager.png)

<span data-ttu-id="2d9b7-127">El **aprobar** y **rechazar** solo se muestran los botones para administradores y administradores.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-127">The **Approve** and **Reject** buttons are only displayed for managers and administrators.</span></span>

<span data-ttu-id="2d9b7-128">En la siguiente imagen, `admin@contoso.com` ha iniciado sesión y en el rol de administrador:</span><span class="sxs-lookup"><span data-stu-id="2d9b7-128">In the following image, `admin@contoso.com` is signed in and in the administrator's role:</span></span>

![Captura de pantalla mostrando admin@contoso.com iniciada](secure-data/_static/admin.png)

<span data-ttu-id="2d9b7-130">El administrador tiene todos los privilegios.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-130">The administrator has all privileges.</span></span> <span data-ttu-id="2d9b7-131">Puede leer, editar o eliminar cualquier contacto y cambiar el estado de los contactos.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-131">She can read/edit/delete any contact and change the status of contacts.</span></span>

<span data-ttu-id="2d9b7-132">Se ha creado la aplicación por [scaffolding](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model) siguiente `Contact` modelo:</span><span class="sxs-lookup"><span data-stu-id="2d9b7-132">The app was created by [scaffolding](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model) the following `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

<span data-ttu-id="2d9b7-133">El ejemplo contiene los siguientes controladores de autorización:</span><span class="sxs-lookup"><span data-stu-id="2d9b7-133">The sample contains the following authorization handlers:</span></span>

* <span data-ttu-id="2d9b7-134">`ContactIsOwnerAuthorizationHandler`: Garantiza que un usuario solo puede editar sus datos.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-134">`ContactIsOwnerAuthorizationHandler`: Ensures that a user can only edit their data.</span></span>
* <span data-ttu-id="2d9b7-135">`ContactManagerAuthorizationHandler`: Permite a los administradores aprobar o rechazar contactos.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-135">`ContactManagerAuthorizationHandler`: Allows managers to approve or reject contacts.</span></span>
* <span data-ttu-id="2d9b7-136">`ContactAdministratorsAuthorizationHandler`: Permite a los administradores aprobar o rechazar contactos y editar o eliminar contactos.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-136">`ContactAdministratorsAuthorizationHandler`: Allows administrators to approve or reject contacts and to edit/delete contacts.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2d9b7-137">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="2d9b7-137">Prerequisites</span></span>

<span data-ttu-id="2d9b7-138">En este tutorial se avanza.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-138">This tutorial is advanced.</span></span> <span data-ttu-id="2d9b7-139">Debe estar familiarizado con:</span><span class="sxs-lookup"><span data-stu-id="2d9b7-139">You should be familiar with:</span></span>

* [<span data-ttu-id="2d9b7-140">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2d9b7-140">ASP.NET Core</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="2d9b7-141">Autenticación</span><span class="sxs-lookup"><span data-stu-id="2d9b7-141">Authentication</span></span>](xref:security/authentication/identity)
* [<span data-ttu-id="2d9b7-142">Confirmación de cuentas y recuperación de contraseñas</span><span class="sxs-lookup"><span data-stu-id="2d9b7-142">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="2d9b7-143">Autorización</span><span class="sxs-lookup"><span data-stu-id="2d9b7-143">Authorization</span></span>](xref:security/authorization/introduction)
* [<span data-ttu-id="2d9b7-144">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="2d9b7-144">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

## <a name="the-starter-and-completed-app"></a><span data-ttu-id="2d9b7-145">El inicio y la aplicación completada</span><span class="sxs-lookup"><span data-stu-id="2d9b7-145">The starter and completed app</span></span>

<span data-ttu-id="2d9b7-146">[Descargar](xref:index#how-to-download-a-sample) el [completado](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples) app.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-146">[Download](xref:index#how-to-download-a-sample) the [completed](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples) app.</span></span> <span data-ttu-id="2d9b7-147">[Prueba](#test-the-completed-app) la aplicación completa, por lo que se familiarice con sus características de seguridad.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-147">[Test](#test-the-completed-app) the completed app so you become familiar with its security features.</span></span>

### <a name="the-starter-app"></a><span data-ttu-id="2d9b7-148">La aplicación de inicio</span><span class="sxs-lookup"><span data-stu-id="2d9b7-148">The starter app</span></span>

<span data-ttu-id="2d9b7-149">[Descargar](xref:index#how-to-download-a-sample) el [starter](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/) app.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-149">[Download](xref:index#how-to-download-a-sample) the [starter](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/) app.</span></span>

<span data-ttu-id="2d9b7-150">Ejecute la aplicación, pulse el **ContactManager** vincular y compruebe que puede crear, editar y eliminar un contacto.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-150">Run the app, tap the **ContactManager** link, and verify you can create, edit, and delete a contact.</span></span>

## <a name="secure-user-data"></a><span data-ttu-id="2d9b7-151">Proteger los datos de usuario</span><span class="sxs-lookup"><span data-stu-id="2d9b7-151">Secure user data</span></span>

<span data-ttu-id="2d9b7-152">Las siguientes secciones contienen todos los pasos principales para crear la aplicación de datos de usuario segura.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-152">The following sections have all the major steps to create the secure user data app.</span></span> <span data-ttu-id="2d9b7-153">Resultarle útil consultar el proyecto completado.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-153">You may find it helpful to refer to the completed project.</span></span>

### <a name="tie-the-contact-data-to-the-user"></a><span data-ttu-id="2d9b7-154">Vincular los datos de contacto para el usuario</span><span class="sxs-lookup"><span data-stu-id="2d9b7-154">Tie the contact data to the user</span></span>

<span data-ttu-id="2d9b7-155">Usar ASP.NET [identidad](xref:security/authentication/identity) Id. de usuario para garantizar que los usuarios puede editar sus datos, pero no otros datos de los usuarios.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-155">Use the ASP.NET [Identity](xref:security/authentication/identity) user ID to ensure users can edit their data, but not other users data.</span></span> <span data-ttu-id="2d9b7-156">Agregar `OwnerID` y `ContactStatus` a la `Contact` modelo:</span><span class="sxs-lookup"><span data-stu-id="2d9b7-156">Add `OwnerID` and `ContactStatus` to the `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/final3/Models/Contact.cs?name=snippet1&highlight=5-6,16-999)]

<span data-ttu-id="2d9b7-157">`OwnerID` es el identificador del usuario desde el `AspNetUser` de tabla en la [identidad](xref:security/authentication/identity) base de datos.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-157">`OwnerID` is the user's ID from the `AspNetUser` table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="2d9b7-158">El `Status` campo determina si un contacto es visible para los usuarios en general.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-158">The `Status` field determines if a contact is viewable by general users.</span></span>

<span data-ttu-id="2d9b7-159">Crear una nueva migración y actualización de la base de datos:</span><span class="sxs-lookup"><span data-stu-id="2d9b7-159">Create a new migration and update the database:</span></span>

```console
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="add-role-services-to-identity"></a><span data-ttu-id="2d9b7-160">Agregar servicios de función a la identidad</span><span class="sxs-lookup"><span data-stu-id="2d9b7-160">Add Role services to Identity</span></span>

<span data-ttu-id="2d9b7-161">Anexar [AddRoles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) para agregar servicios de rol:</span><span class="sxs-lookup"><span data-stu-id="2d9b7-161">Append [AddRoles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) to add Role services:</span></span>

[!code-csharp[](secure-data/samples/final3/Startup.cs?name=snippet2&highlight=9)]

### <a name="require-authenticated-users"></a><span data-ttu-id="2d9b7-162">Requerir que los usuarios autenticados</span><span class="sxs-lookup"><span data-stu-id="2d9b7-162">Require authenticated users</span></span>

<span data-ttu-id="2d9b7-163">Establezca la directiva de autenticación predeterminado para exigir que los usuarios se autentiquen:</span><span class="sxs-lookup"><span data-stu-id="2d9b7-163">Set the default authentication policy to require users to be authenticated:</span></span>

[!code-csharp[](secure-data/samples/final3/Startup.cs?name=snippet&highlight=15-99)] 

 <span data-ttu-id="2d9b7-164">Puede rechazar la autenticación en el nivel de método de página de Razor, controlador o acción con el `[AllowAnonymous]` atributo.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-164">You can opt out of authentication at the Razor Page, controller, or action method level with the `[AllowAnonymous]` attribute.</span></span> <span data-ttu-id="2d9b7-165">Configuración de la directiva de autenticación predeterminado para exigir que los usuarios se autentiquen protege recién agregada de las páginas de Razor y controladores.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-165">Setting the default authentication policy to require users to be authenticated protects newly added Razor Pages and controllers.</span></span> <span data-ttu-id="2d9b7-166">Tener la autenticación requerida por el valor predeterminado es más segura que confiar en los nuevos controladores y las páginas de Razor debe incluir el `[Authorize]` atributo.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-166">Having authentication required by default is more secure than relying on new controllers and Razor Pages to include the `[Authorize]` attribute.</span></span>

<span data-ttu-id="2d9b7-167">Agregue [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) a las páginas de índice y privacidad para que los usuarios anónimos puedan obtener información sobre el sitio antes de registrarse.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-167">Add [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) to the Index and Privacy pages so anonymous users can get information about the site before they register.</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Index.cshtml.cs?highlight=1,7)]

### <a name="configure-the-test-account"></a><span data-ttu-id="2d9b7-168">Configurar la cuenta de prueba</span><span class="sxs-lookup"><span data-stu-id="2d9b7-168">Configure the test account</span></span>

<span data-ttu-id="2d9b7-169">La `SeedData` clase crea dos cuentas: administrador y administrador.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-169">The `SeedData` class creates two accounts: administrator and manager.</span></span> <span data-ttu-id="2d9b7-170">Use la [herramienta Secret Manager](xref:security/app-secrets) para establecer una contraseña para estas cuentas.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-170">Use the [Secret Manager tool](xref:security/app-secrets) to set a password for these accounts.</span></span> <span data-ttu-id="2d9b7-171">Establecer la contraseña del directorio de proyecto (el directorio que contiene *Program.cs*):</span><span class="sxs-lookup"><span data-stu-id="2d9b7-171">Set the password from the project directory (the directory containing *Program.cs*):</span></span>

```console
dotnet user-secrets set SeedUserPW <PW>
```

<span data-ttu-id="2d9b7-172">Si no se especifica una contraseña segura, se produce una excepción cuando `SeedData.Initialize` se llama.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-172">If a strong password is not specified, an exception is thrown when `SeedData.Initialize` is called.</span></span>

<span data-ttu-id="2d9b7-173">Actualización `Main` usar la contraseña de la prueba:</span><span class="sxs-lookup"><span data-stu-id="2d9b7-173">Update `Main` to use the test password:</span></span>

[!code-csharp[](secure-data/samples/final3/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a><span data-ttu-id="2d9b7-174">Crear las cuentas de prueba y actualizar los contactos</span><span class="sxs-lookup"><span data-stu-id="2d9b7-174">Create the test accounts and update the contacts</span></span>

<span data-ttu-id="2d9b7-175">Actualización de la `Initialize` método en el `SeedData` clase para crear las cuentas de prueba:</span><span class="sxs-lookup"><span data-stu-id="2d9b7-175">Update the `Initialize` method in the `SeedData` class to create the test accounts:</span></span>

[!code-csharp[](secure-data/samples/final3/Data/SeedData.cs?name=snippet_Initialize)]

<span data-ttu-id="2d9b7-176">Agregue el identificador de usuario administrador y `ContactStatus` a los contactos.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-176">Add the administrator user ID and `ContactStatus` to the contacts.</span></span> <span data-ttu-id="2d9b7-177">Realice uno de los contactos "Enviado" y un "rechazada".</span><span class="sxs-lookup"><span data-stu-id="2d9b7-177">Make one of the contacts "Submitted" and one "Rejected".</span></span> <span data-ttu-id="2d9b7-178">Agregue el Id. de usuario y el estado a todos los contactos.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-178">Add the user ID and status to all the contacts.</span></span> <span data-ttu-id="2d9b7-179">Póngase en contacto un solo con se muestra:</span><span class="sxs-lookup"><span data-stu-id="2d9b7-179">Only one contact is shown:</span></span>

[!code-csharp[](secure-data/samples/final3/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a><span data-ttu-id="2d9b7-180">Crear controladores de autorización de administrador, administrador y propietario</span><span class="sxs-lookup"><span data-stu-id="2d9b7-180">Create owner, manager, and administrator authorization handlers</span></span>

<span data-ttu-id="2d9b7-181">Crear un `ContactIsOwnerAuthorizationHandler` clase en el *autorización* carpeta.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-181">Create a `ContactIsOwnerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="2d9b7-182">El `ContactIsOwnerAuthorizationHandler` comprueba que el usuario que actúan en un recurso posee el recurso.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-182">The `ContactIsOwnerAuthorizationHandler` verifies that the user acting on a resource owns the resource.</span></span>

[!code-csharp[](secure-data/samples/final3/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

<span data-ttu-id="2d9b7-183">El `ContactIsOwnerAuthorizationHandler` llamadas [contexto. Se realizan correctamente](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) si el usuario autenticado actual es el propietario del contacto.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-183">The `ContactIsOwnerAuthorizationHandler` calls [context.Succeed](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) if the current authenticated user is the contact owner.</span></span> <span data-ttu-id="2d9b7-184">Controladores de autorización con carácter general:</span><span class="sxs-lookup"><span data-stu-id="2d9b7-184">Authorization handlers generally:</span></span>

* <span data-ttu-id="2d9b7-185">Devolver `context.Succeed` cuando se cumplen los requisitos.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-185">Return `context.Succeed` when the requirements are met.</span></span>
* <span data-ttu-id="2d9b7-186">Devolver `Task.CompletedTask` cuando no se cumplen los requisitos.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-186">Return `Task.CompletedTask` when requirements aren't met.</span></span> <span data-ttu-id="2d9b7-187">`Task.CompletedTask`no es correcto o erróneo&mdash;permite que se ejecuten otros controladores de autorización.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-187">`Task.CompletedTask` is not success or failure&mdash;it allows other authorization handlers to run.</span></span>

<span data-ttu-id="2d9b7-188">Si necesita explícitamente un error, devolver [contexto. Un error](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span><span class="sxs-lookup"><span data-stu-id="2d9b7-188">If you need to explicitly fail, return [context.Fail](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span></span>

<span data-ttu-id="2d9b7-189">La aplicación permite a los propietarios de contacto para editar, eliminar o crear sus propios datos.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-189">The app allows contact owners to edit/delete/create their own data.</span></span> <span data-ttu-id="2d9b7-190">`ContactIsOwnerAuthorizationHandler` no es necesario comprobar la operación que se pasa en el parámetro de requisito.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-190">`ContactIsOwnerAuthorizationHandler` doesn't need to check the operation passed in the requirement parameter.</span></span>

### <a name="create-a-manager-authorization-handler"></a><span data-ttu-id="2d9b7-191">Crear un controlador de autorización de administrador</span><span class="sxs-lookup"><span data-stu-id="2d9b7-191">Create a manager authorization handler</span></span>

<span data-ttu-id="2d9b7-192">Crear un `ContactManagerAuthorizationHandler` clase en el *autorización* carpeta.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-192">Create a `ContactManagerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="2d9b7-193">El `ContactManagerAuthorizationHandler` comprueba que el usuario que actúan en el recurso es un administrador.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-193">The `ContactManagerAuthorizationHandler` verifies the user acting on the resource is a manager.</span></span> <span data-ttu-id="2d9b7-194">Solo los administradores pueden aprobar o rechazar los cambios de contenido (nuevos o modificados).</span><span class="sxs-lookup"><span data-stu-id="2d9b7-194">Only managers can approve or reject content changes (new or changed).</span></span>

[!code-csharp[](secure-data/samples/final3/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a><span data-ttu-id="2d9b7-195">Cree un controlador de autorización de administrador</span><span class="sxs-lookup"><span data-stu-id="2d9b7-195">Create an administrator authorization handler</span></span>

<span data-ttu-id="2d9b7-196">Crear un `ContactAdministratorsAuthorizationHandler` clase en el *autorización* carpeta.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-196">Create a `ContactAdministratorsAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="2d9b7-197">El `ContactAdministratorsAuthorizationHandler` comprueba que el usuario que actúan en el recurso es un administrador.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-197">The `ContactAdministratorsAuthorizationHandler` verifies the user acting on the resource is an administrator.</span></span> <span data-ttu-id="2d9b7-198">Administrador puede realizar todas las operaciones.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-198">Administrator can do all operations.</span></span>

[!code-csharp[](secure-data/samples/final3/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a><span data-ttu-id="2d9b7-199">Registrar los controladores de autorización</span><span class="sxs-lookup"><span data-stu-id="2d9b7-199">Register the authorization handlers</span></span>

<span data-ttu-id="2d9b7-200">Se deben registrar los servicios mediante Entity Framework Core para [inserción de dependencias](xref:fundamentals/dependency-injection) mediante [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span><span class="sxs-lookup"><span data-stu-id="2d9b7-200">Services using Entity Framework Core must be registered for [dependency injection](xref:fundamentals/dependency-injection) using [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span></span> <span data-ttu-id="2d9b7-201">El `ContactIsOwnerAuthorizationHandler` utiliza ASP.NET Core [identidad](xref:security/authentication/identity), que se basa en Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-201">The `ContactIsOwnerAuthorizationHandler` uses ASP.NET Core [Identity](xref:security/authentication/identity), which is built on Entity Framework Core.</span></span> <span data-ttu-id="2d9b7-202">Registre los controladores con la colección de servicios para que estén disponibles para el `ContactsController` a través de [inserción de dependencias](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="2d9b7-202">Register the handlers with the service collection so they're available to the `ContactsController` through [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="2d9b7-203">Agregue el código siguiente al final de `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="2d9b7-203">Add the following code to the end of `ConfigureServices`:</span></span>

[!code-csharp[](secure-data/samples/final3/Startup.cs?name=snippet_defaultPolicy&highlight=23-99)]

<span data-ttu-id="2d9b7-204">`ContactAdministratorsAuthorizationHandler` y `ContactManagerAuthorizationHandler` se agregan como singleton.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-204">`ContactAdministratorsAuthorizationHandler` and `ContactManagerAuthorizationHandler` are added as singletons.</span></span> <span data-ttu-id="2d9b7-205">Lo singletons con estado porque no usan EF y toda la información necesaria se encuentra en la `Context` parámetro de la `HandleRequirementAsync` método.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-205">They're singletons because they don't use EF and all the information needed is in the `Context` parameter of the `HandleRequirementAsync` method.</span></span>

## <a name="support-authorization"></a><span data-ttu-id="2d9b7-206">Admitir la autorización</span><span class="sxs-lookup"><span data-stu-id="2d9b7-206">Support authorization</span></span>

<span data-ttu-id="2d9b7-207">En esta sección, actualice las páginas de Razor y agregue una clase de los requisitos de operaciones.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-207">In this section, you update the Razor Pages and add an operations requirements class.</span></span>

### <a name="review-the-contact-operations-requirements-class"></a><span data-ttu-id="2d9b7-208">Revisión de la clase de los requisitos de las operaciones de contacto</span><span class="sxs-lookup"><span data-stu-id="2d9b7-208">Review the contact operations requirements class</span></span>

<span data-ttu-id="2d9b7-209">Revise el `ContactOperations` clase.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-209">Review the `ContactOperations` class.</span></span> <span data-ttu-id="2d9b7-210">Esta clase contiene los requisitos que admite la aplicación:</span><span class="sxs-lookup"><span data-stu-id="2d9b7-210">This class contains the requirements the app supports:</span></span>

[!code-csharp[](secure-data/samples/final3/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-contacts-razor-pages"></a><span data-ttu-id="2d9b7-211">Crear una clase base para las páginas de Razor de contactos</span><span class="sxs-lookup"><span data-stu-id="2d9b7-211">Create a base class for the Contacts Razor Pages</span></span>

<span data-ttu-id="2d9b7-212">Cree una clase base que contiene los servicios usados en los contactos de las páginas de Razor.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-212">Create a base class that contains the services used in the contacts Razor Pages.</span></span> <span data-ttu-id="2d9b7-213">La clase base coloca el código de inicialización en una ubicación:</span><span class="sxs-lookup"><span data-stu-id="2d9b7-213">The base class puts the initialization code in one location:</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/DI_BasePageModel.cs)]

<span data-ttu-id="2d9b7-214">El código anterior:</span><span class="sxs-lookup"><span data-stu-id="2d9b7-214">The preceding code:</span></span>

* <span data-ttu-id="2d9b7-215">Agrega el `IAuthorizationService` service acceda a los controladores de autorización.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-215">Adds the `IAuthorizationService` service to access to the authorization handlers.</span></span>
* <span data-ttu-id="2d9b7-216">Agrega la identidad `UserManager` service.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-216">Adds the Identity `UserManager` service.</span></span>
* <span data-ttu-id="2d9b7-217">Agregue la `ApplicationDbContext`.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-217">Add the `ApplicationDbContext`.</span></span>

### <a name="update-the-createmodel"></a><span data-ttu-id="2d9b7-218">Actualizar el CreateModel</span><span class="sxs-lookup"><span data-stu-id="2d9b7-218">Update the CreateModel</span></span>

<span data-ttu-id="2d9b7-219">Actualizar el constructor del modelo de página create para usar el `DI_BasePageModel` clase base:</span><span class="sxs-lookup"><span data-stu-id="2d9b7-219">Update the create page model constructor to use the `DI_BasePageModel` base class:</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

<span data-ttu-id="2d9b7-220">Actualización de la `CreateModel.OnPostAsync` método:</span><span class="sxs-lookup"><span data-stu-id="2d9b7-220">Update the `CreateModel.OnPostAsync` method to:</span></span>

* <span data-ttu-id="2d9b7-221">Agregue el identificador de usuario para el `Contact` modelo.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-221">Add the user ID to the `Contact` model.</span></span>
* <span data-ttu-id="2d9b7-222">Llamar al controlador de autorización para comprobar que el usuario tiene permiso para crear contactos.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-222">Call the authorization handler to verify the user has permission to create contacts.</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a><span data-ttu-id="2d9b7-223">Actualizar el IndexModel</span><span class="sxs-lookup"><span data-stu-id="2d9b7-223">Update the IndexModel</span></span>

<span data-ttu-id="2d9b7-224">Actualización de la `OnGetAsync` método por lo que solo contactos aprobados se muestran a los usuarios generales:</span><span class="sxs-lookup"><span data-stu-id="2d9b7-224">Update the `OnGetAsync` method so only approved contacts are shown to general users:</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a><span data-ttu-id="2d9b7-225">Actualizar el EditModel</span><span class="sxs-lookup"><span data-stu-id="2d9b7-225">Update the EditModel</span></span>

<span data-ttu-id="2d9b7-226">Agregar un controlador de autorización para comprobar que el usuario propietario del contacto.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-226">Add an authorization handler to verify the user owns the contact.</span></span> <span data-ttu-id="2d9b7-227">Dado que se está validando la autorización de recursos, el `[Authorize]` atributo no es suficiente.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-227">Because resource authorization is being validated, the `[Authorize]` attribute is not enough.</span></span> <span data-ttu-id="2d9b7-228">La aplicación no tiene acceso al recurso cuando se evalúan los atributos.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-228">The app doesn't have access to the resource when attributes are evaluated.</span></span> <span data-ttu-id="2d9b7-229">Autorización basada en el recurso debe ser un imperativo.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-229">Resource-based authorization must be imperative.</span></span> <span data-ttu-id="2d9b7-230">Las comprobaciones deben realizarse una vez que la aplicación tenga acceso al recurso, cargándolo en el modelo de página o cargándola en el controlador de sí mismo.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-230">Checks must be performed once the app has access to the resource, either by loading it in the page model or by loading it within the handler itself.</span></span> <span data-ttu-id="2d9b7-231">Con frecuencia acceder al recurso pasando la clave de recurso.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-231">You frequently access the resource by passing in the resource key.</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a><span data-ttu-id="2d9b7-232">Actualizar el DeleteModel</span><span class="sxs-lookup"><span data-stu-id="2d9b7-232">Update the DeleteModel</span></span>

<span data-ttu-id="2d9b7-233">Actualice el modelo de página de delete para usar el controlador de autorización para comprobar que el usuario tiene permiso delete en el contacto.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-233">Update the delete page model to use the authorization handler to verify the user has delete permission on the contact.</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a><span data-ttu-id="2d9b7-234">Insertar el servicio de autorización en las vistas</span><span class="sxs-lookup"><span data-stu-id="2d9b7-234">Inject the authorization service into the views</span></span>

<span data-ttu-id="2d9b7-235">Actualmente, se muestra en la interfaz de usuario edita y elimina los vínculos para los contactos que no se puede modificar el usuario.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-235">Currently, the UI shows edit and delete links for contacts the user can't modify.</span></span>

<span data-ttu-id="2d9b7-236">Inserte el servicio de autorización en el archivo *pages/_ViewImports. cshtml* para que esté disponible para todas las vistas:</span><span class="sxs-lookup"><span data-stu-id="2d9b7-236">Inject the authorization service in the *Pages/_ViewImports.cshtml* file so it's available to all views:</span></span>

[!code-cshtml[](secure-data/samples/final3/Pages/_ViewImports.cshtml?highlight=6-99)]

<span data-ttu-id="2d9b7-237">El marcado anterior agrega varios `using` instrucciones.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-237">The preceding markup adds several `using` statements.</span></span>

<span data-ttu-id="2d9b7-238">Actualización de la **editar** y **eliminar** vincula en *Pages/Contacts/Index.cshtml* por lo que sólo se representen a los usuarios con los permisos adecuados:</span><span class="sxs-lookup"><span data-stu-id="2d9b7-238">Update the **Edit** and **Delete** links in *Pages/Contacts/Index.cshtml* so they're only rendered for users with the appropriate permissions:</span></span>

[!code-cshtml[](secure-data/samples/final3/Pages/Contacts/Index.cshtml?highlight=34-36,62-999)]

> [!WARNING]
> <span data-ttu-id="2d9b7-239">Ocultar los vínculos de los usuarios que no tienen permiso para cambiar los datos no proteger la aplicación.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-239">Hiding links from users that don't have permission to change data doesn't secure the app.</span></span> <span data-ttu-id="2d9b7-240">Ocultar vínculos hace que la aplicación más fácil de usar mostrando vínculos solo es válido.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-240">Hiding links makes the app more user-friendly by displaying only valid links.</span></span> <span data-ttu-id="2d9b7-241">Los usuarios pueden hack las direcciones URL generadas para invocar Editar y eliminar operaciones en los datos que no poseen.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-241">Users can hack the generated URLs to invoke edit and delete operations on data they don't own.</span></span> <span data-ttu-id="2d9b7-242">La página de Razor o el controlador debe exigir comprobaciones de acceso para proteger los datos.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-242">The Razor Page or controller must enforce access checks to secure the data.</span></span>

### <a name="update-details"></a><span data-ttu-id="2d9b7-243">Detalles de la actualización</span><span class="sxs-lookup"><span data-stu-id="2d9b7-243">Update Details</span></span>

<span data-ttu-id="2d9b7-244">Actualice la vista de detalles para que los administradores pueden aprobar o rechazar contactos:</span><span class="sxs-lookup"><span data-stu-id="2d9b7-244">Update the details view so managers can approve or reject contacts:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml?name=snippet)]

<span data-ttu-id="2d9b7-245">Actualice el modelo de página de detalles:</span><span class="sxs-lookup"><span data-stu-id="2d9b7-245">Update the details page model:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="add-or-remove-a-user-to-a-role"></a><span data-ttu-id="2d9b7-246">Agregar o quitar un usuario a un rol</span><span class="sxs-lookup"><span data-stu-id="2d9b7-246">Add or remove a user to a role</span></span>

<span data-ttu-id="2d9b7-247">Consulte [este problema](https://github.com/aspnet/AspNetCore.Docs/issues/8502) para obtener información sobre:</span><span class="sxs-lookup"><span data-stu-id="2d9b7-247">See [this issue](https://github.com/aspnet/AspNetCore.Docs/issues/8502) for information on:</span></span>

* <span data-ttu-id="2d9b7-248">Quita los privilegios de un usuario.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-248">Removing privileges from a user.</span></span> <span data-ttu-id="2d9b7-249">Por ejemplo, silenciar a un usuario en una aplicación de chat.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-249">For example, muting a user in a chat app.</span></span>
* <span data-ttu-id="2d9b7-250">Agregar privilegios a un usuario.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-250">Adding privileges to a user.</span></span>

## <a name="test-the-completed-app"></a><span data-ttu-id="2d9b7-251">Probar la aplicación completada</span><span class="sxs-lookup"><span data-stu-id="2d9b7-251">Test the completed app</span></span>

<span data-ttu-id="2d9b7-252">Si ya no ha establecido una contraseña para cuentas de usuario inicializados, utilice el [herramienta Secret Manager](xref:security/app-secrets#secret-manager) para establecer una contraseña:</span><span class="sxs-lookup"><span data-stu-id="2d9b7-252">If you haven't already set a password for seeded user accounts, use the [Secret Manager tool](xref:security/app-secrets#secret-manager) to set a password:</span></span>

* <span data-ttu-id="2d9b7-253">Elija una contraseña segura: Use ocho o más caracteres y al menos un carácter, número y símbolo en mayúsculas.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-253">Choose a strong password: Use eight or more characters and at least one upper-case character, number, and symbol.</span></span> <span data-ttu-id="2d9b7-254">Por ejemplo, `Passw0rd!` cumple los requisitos de contraseña segura.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-254">For example, `Passw0rd!` meets the strong password requirements.</span></span>
* <span data-ttu-id="2d9b7-255">Ejecute el siguiente comando desde la carpeta del proyecto, donde `<PW>` es la contraseña:</span><span class="sxs-lookup"><span data-stu-id="2d9b7-255">Execute the following command from the project's folder, where `<PW>` is the password:</span></span>

  ```console
  dotnet user-secrets set SeedUserPW <PW>
  ```

<span data-ttu-id="2d9b7-256">Si la aplicación tiene contactos:</span><span class="sxs-lookup"><span data-stu-id="2d9b7-256">If the app has contacts:</span></span>

* <span data-ttu-id="2d9b7-257">Eliminar todos los registros en el `Contact` tabla.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-257">Delete all of the records in the `Contact` table.</span></span>
* <span data-ttu-id="2d9b7-258">Reinicie la aplicación para inicializar la base de datos.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-258">Restart the app to seed the database.</span></span>

<span data-ttu-id="2d9b7-259">Es una manera fácil de probar la aplicación completa iniciar los tres distintos exploradores (o las sesiones de InPrivate o incognito).</span><span class="sxs-lookup"><span data-stu-id="2d9b7-259">An easy way to test the completed app is to launch three different browsers (or incognito/InPrivate sessions).</span></span> <span data-ttu-id="2d9b7-260">En un explorador, registre un nuevo usuario (por ejemplo, `test@example.com`).</span><span class="sxs-lookup"><span data-stu-id="2d9b7-260">In one browser, register a new user (for example, `test@example.com`).</span></span> <span data-ttu-id="2d9b7-261">Inicie sesión con un usuario diferente en cada explorador.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-261">Sign in to each browser with a different user.</span></span> <span data-ttu-id="2d9b7-262">Compruebe las siguientes operaciones:</span><span class="sxs-lookup"><span data-stu-id="2d9b7-262">Verify the following operations:</span></span>

* <span data-ttu-id="2d9b7-263">Usuarios registrados pueden ver todos los datos de contacto aprobados.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-263">Registered users can view all of the approved contact data.</span></span>
* <span data-ttu-id="2d9b7-264">Los usuarios registrados pueden editar o eliminar sus propios datos.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-264">Registered users can edit/delete their own data.</span></span>
* <span data-ttu-id="2d9b7-265">Los administradores pueden aprobar o rechazar datos de contacto.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-265">Managers can approve/reject contact data.</span></span> <span data-ttu-id="2d9b7-266">El `Details` ver muestra **aprobar** y **rechazar** botones.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-266">The `Details` view shows **Approve** and **Reject** buttons.</span></span>
* <span data-ttu-id="2d9b7-267">Los administradores pueden aprobar o rechazar y editar o eliminar todos los datos.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-267">Administrators can approve/reject and edit/delete all data.</span></span>

| <span data-ttu-id="2d9b7-268">Usuario</span><span class="sxs-lookup"><span data-stu-id="2d9b7-268">User</span></span>                | <span data-ttu-id="2d9b7-269">Propagadas por la aplicación</span><span class="sxs-lookup"><span data-stu-id="2d9b7-269">Seeded by the app</span></span> | <span data-ttu-id="2d9b7-270">Opciones</span><span class="sxs-lookup"><span data-stu-id="2d9b7-270">Options</span></span>                                  |
| ------------------- | :---------------: | ---------------------------------------- |
| test@example.com    | <span data-ttu-id="2d9b7-271">No</span><span class="sxs-lookup"><span data-stu-id="2d9b7-271">No</span></span>                | <span data-ttu-id="2d9b7-272">Editar o eliminar los datos propios.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-272">Edit/delete the own data.</span></span>                |
| manager@contoso.com | <span data-ttu-id="2d9b7-273">Sí</span><span class="sxs-lookup"><span data-stu-id="2d9b7-273">Yes</span></span>               | <span data-ttu-id="2d9b7-274">Aprobar o rechazar y editar o eliminar los datos propios.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-274">Approve/reject and edit/delete own data.</span></span> |
| admin@contoso.com   | <span data-ttu-id="2d9b7-275">Sí</span><span class="sxs-lookup"><span data-stu-id="2d9b7-275">Yes</span></span>               | <span data-ttu-id="2d9b7-276">Aprobar o rechazar y editar o eliminar todos los datos.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-276">Approve/reject and edit/delete all data.</span></span> |

<span data-ttu-id="2d9b7-277">Crear un contacto en el explorador del administrador.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-277">Create a contact in the administrator's browser.</span></span> <span data-ttu-id="2d9b7-278">Copie la dirección URL para su eliminación y editar en el contacto del administrador.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-278">Copy the URL for delete and edit from the administrator contact.</span></span> <span data-ttu-id="2d9b7-279">Pegue estos vínculos en el explorador del usuario de prueba para comprobar que el usuario de prueba no puede realizar estas operaciones.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-279">Paste these links into the test user's browser to verify the test user can't perform these operations.</span></span>

## <a name="create-the-starter-app"></a><span data-ttu-id="2d9b7-280">Crear la aplicación de inicio</span><span class="sxs-lookup"><span data-stu-id="2d9b7-280">Create the starter app</span></span>

* <span data-ttu-id="2d9b7-281">Cree una aplicación de páginas de Razor denominada "ContactManager"</span><span class="sxs-lookup"><span data-stu-id="2d9b7-281">Create a Razor Pages app named "ContactManager"</span></span>
  * <span data-ttu-id="2d9b7-282">Creación de la aplicación con **cuentas de usuario individuales**.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-282">Create the app with **Individual User Accounts**.</span></span>
  * <span data-ttu-id="2d9b7-283">Asígnele el nombre "ContactManager" para el espacio de nombres coincida con el espacio de nombres utilizado en el ejemplo.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-283">Name it "ContactManager" so the namespace matches the namespace used in the sample.</span></span>
  * <span data-ttu-id="2d9b7-284">`-uld` Especifica LocalDB en lugar de SQLite</span><span class="sxs-lookup"><span data-stu-id="2d9b7-284">`-uld` specifies LocalDB instead of SQLite</span></span>

  ```console
  dotnet new webapp -o ContactManager -au Individual -uld
  ```

* <span data-ttu-id="2d9b7-285">Agregue *Models/contact. CS*:</span><span class="sxs-lookup"><span data-stu-id="2d9b7-285">Add *Models/Contact.cs*:</span></span>

  [!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

* <span data-ttu-id="2d9b7-286">Scaffold el `Contact` modelo.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-286">Scaffold the `Contact` model.</span></span>
* <span data-ttu-id="2d9b7-287">Crear la migración inicial y actualizar la base de datos:</span><span class="sxs-lookup"><span data-stu-id="2d9b7-287">Create initial migration and update the database:</span></span>

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet tool install -g dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
dotnet ef database drop -f
dotnet ef migrations add initial
dotnet ef database update
  ```

<span data-ttu-id="2d9b7-288">Si experimenta un error con el `dotnet aspnet-codegenerator razorpage` comando, consulte [este problema de github](https://github.com/aspnet/Scaffolding/issues/984).</span><span class="sxs-lookup"><span data-stu-id="2d9b7-288">If you experience a bug with the `dotnet aspnet-codegenerator razorpage` command, see [this GitHub issue](https://github.com/aspnet/Scaffolding/issues/984).</span></span>

* <span data-ttu-id="2d9b7-289">Actualice el delimitador de **ContactManager** en el archivo *pages/Shared/_Layout. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="2d9b7-289">Update the **ContactManager** anchor in the *Pages/Shared/_Layout.cshtml* file:</span></span>

 ```cshtml
<a class="navbar-brand" asp-area="" asp-page="/Contacts/Index">ContactManager</a>
  ```

* <span data-ttu-id="2d9b7-290">Probar la aplicación mediante la creación, edición y eliminación de un contacto</span><span class="sxs-lookup"><span data-stu-id="2d9b7-290">Test the app by creating, editing, and deleting a contact</span></span>

### <a name="seed-the-database"></a><span data-ttu-id="2d9b7-291">Inicializar la base de datos</span><span class="sxs-lookup"><span data-stu-id="2d9b7-291">Seed the database</span></span>

<span data-ttu-id="2d9b7-292">Agregue la clase [SeedData](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter3/Data/SeedData.cs) a la carpeta de *datos* :</span><span class="sxs-lookup"><span data-stu-id="2d9b7-292">Add the [SeedData](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter3/Data/SeedData.cs) class to the *Data* folder:</span></span>

[!code-csharp[](secure-data/samples/starter3/Data/SeedData.cs)]

<span data-ttu-id="2d9b7-293">Llame a `SeedData.Initialize` desde `Main`:</span><span class="sxs-lookup"><span data-stu-id="2d9b7-293">Call `SeedData.Initialize` from `Main`:</span></span>

[!code-csharp[](secure-data/samples/starter3/Program.cs)]

<span data-ttu-id="2d9b7-294">Probar que la aplicación había propagado la base de datos.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-294">Test that the app seeded the database.</span></span> <span data-ttu-id="2d9b7-295">Si no hay ninguna fila en la base de datos de contacto, no se ejecuta el método de inicialización.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-295">If there are any rows in the contact DB, the seed method doesn't run.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1 < aspnetcore-3.0"

<span data-ttu-id="2d9b7-296">Este tutorial muestra cómo crear una aplicación web ASP.NET Core con los datos protegidos por autorización del usuario.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-296">This tutorial shows how to create an ASP.NET Core web app with user data protected by authorization.</span></span> <span data-ttu-id="2d9b7-297">Muestra una lista de contactos que autentican los usuarios (registrados) ha creado.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-297">It displays a list of contacts that authenticated (registered) users have created.</span></span> <span data-ttu-id="2d9b7-298">Hay tres grupos de seguridad:</span><span class="sxs-lookup"><span data-stu-id="2d9b7-298">There are three security groups:</span></span>

* <span data-ttu-id="2d9b7-299">**Usuarios registrados** puede ver todos los datos aprobados y pueden sus propios datos de editar o eliminar.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-299">**Registered users** can view all the approved data and can edit/delete their own data.</span></span>
* <span data-ttu-id="2d9b7-300">**Los administradores de** puede aprobar o rechazar los datos de contacto.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-300">**Managers** can approve or reject contact data.</span></span> <span data-ttu-id="2d9b7-301">Solo contactos aprobados son visibles para los usuarios.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-301">Only approved contacts are visible to users.</span></span>
* <span data-ttu-id="2d9b7-302">**Los administradores** puede aprobar o rechazar y editar o eliminar todos los datos.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-302">**Administrators** can approve/reject and edit/delete any data.</span></span>

<span data-ttu-id="2d9b7-303">En la siguiente imagen, el usuario Rick (`rick@example.com`) ha iniciado sesión.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-303">In the following image, user Rick (`rick@example.com`) is signed in.</span></span> <span data-ttu-id="2d9b7-304">Rick solo puede ver los contactos aprobados y **editar**/**eliminar**/**crear nuevo** vínculos para sus contactos.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-304">Rick can only view approved contacts and **Edit**/**Delete**/**Create New** links for his contacts.</span></span> <span data-ttu-id="2d9b7-305">El último registro, creado por Rick, muestra **editar** y **eliminar** vínculos.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-305">Only the last record, created by Rick, displays **Edit** and **Delete** links.</span></span> <span data-ttu-id="2d9b7-306">Otros usuarios no verán el último registro hasta un administrador o un administrador cambia el estado a "Aprobado".</span><span class="sxs-lookup"><span data-stu-id="2d9b7-306">Other users won't see the last record until a manager or administrator changes the status to "Approved".</span></span>

![Captura de pantalla con Rick ha iniciado sesión](secure-data/_static/rick.png)

<span data-ttu-id="2d9b7-308">En la siguiente imagen, `manager@contoso.com` ha iniciado sesión y en el rol del administrador:</span><span class="sxs-lookup"><span data-stu-id="2d9b7-308">In the following image, `manager@contoso.com` is signed in and in the manager's role:</span></span>

![Captura de pantalla mostrando manager@contoso.com iniciada](secure-data/_static/manager1.png)

<span data-ttu-id="2d9b7-310">La siguiente imagen muestra a los administradores de la vista de detalles de un contacto:</span><span class="sxs-lookup"><span data-stu-id="2d9b7-310">The following image shows the managers details view of a contact:</span></span>

![Vista del Administrador de un contacto](secure-data/_static/manager.png)

<span data-ttu-id="2d9b7-312">El **aprobar** y **rechazar** solo se muestran los botones para administradores y administradores.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-312">The **Approve** and **Reject** buttons are only displayed for managers and administrators.</span></span>

<span data-ttu-id="2d9b7-313">En la siguiente imagen, `admin@contoso.com` ha iniciado sesión y en el rol de administrador:</span><span class="sxs-lookup"><span data-stu-id="2d9b7-313">In the following image, `admin@contoso.com` is signed in and in the administrator's role:</span></span>

![Captura de pantalla mostrando admin@contoso.com iniciada](secure-data/_static/admin.png)

<span data-ttu-id="2d9b7-315">El administrador tiene todos los privilegios.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-315">The administrator has all privileges.</span></span> <span data-ttu-id="2d9b7-316">Puede leer, editar o eliminar cualquier contacto y cambiar el estado de los contactos.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-316">She can read/edit/delete any contact and change the status of contacts.</span></span>

<span data-ttu-id="2d9b7-317">Se ha creado la aplicación por [scaffolding](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model) siguiente `Contact` modelo:</span><span class="sxs-lookup"><span data-stu-id="2d9b7-317">The app was created by [scaffolding](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model) the following `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

<span data-ttu-id="2d9b7-318">El ejemplo contiene los siguientes controladores de autorización:</span><span class="sxs-lookup"><span data-stu-id="2d9b7-318">The sample contains the following authorization handlers:</span></span>

* <span data-ttu-id="2d9b7-319">`ContactIsOwnerAuthorizationHandler`: Garantiza que un usuario solo puede editar sus datos.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-319">`ContactIsOwnerAuthorizationHandler`: Ensures that a user can only edit their data.</span></span>
* <span data-ttu-id="2d9b7-320">`ContactManagerAuthorizationHandler`: Permite a los administradores aprobar o rechazar contactos.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-320">`ContactManagerAuthorizationHandler`: Allows managers to approve or reject contacts.</span></span>
* <span data-ttu-id="2d9b7-321">`ContactAdministratorsAuthorizationHandler`: Permite a los administradores aprobar o rechazar contactos y editar o eliminar contactos.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-321">`ContactAdministratorsAuthorizationHandler`: Allows administrators to approve or reject contacts and to edit/delete contacts.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2d9b7-322">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="2d9b7-322">Prerequisites</span></span>

<span data-ttu-id="2d9b7-323">En este tutorial se avanza.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-323">This tutorial is advanced.</span></span> <span data-ttu-id="2d9b7-324">Debe estar familiarizado con:</span><span class="sxs-lookup"><span data-stu-id="2d9b7-324">You should be familiar with:</span></span>

* [<span data-ttu-id="2d9b7-325">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2d9b7-325">ASP.NET Core</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="2d9b7-326">Autenticación</span><span class="sxs-lookup"><span data-stu-id="2d9b7-326">Authentication</span></span>](xref:security/authentication/identity)
* [<span data-ttu-id="2d9b7-327">Confirmación de cuentas y recuperación de contraseñas</span><span class="sxs-lookup"><span data-stu-id="2d9b7-327">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="2d9b7-328">Autorización</span><span class="sxs-lookup"><span data-stu-id="2d9b7-328">Authorization</span></span>](xref:security/authorization/introduction)
* [<span data-ttu-id="2d9b7-329">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="2d9b7-329">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

## <a name="the-starter-and-completed-app"></a><span data-ttu-id="2d9b7-330">El inicio y la aplicación completada</span><span class="sxs-lookup"><span data-stu-id="2d9b7-330">The starter and completed app</span></span>

<span data-ttu-id="2d9b7-331">[Descargar](xref:index#how-to-download-a-sample) el [completado](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples) app.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-331">[Download](xref:index#how-to-download-a-sample) the [completed](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples) app.</span></span> <span data-ttu-id="2d9b7-332">[Prueba](#test-the-completed-app) la aplicación completa, por lo que se familiarice con sus características de seguridad.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-332">[Test](#test-the-completed-app) the completed app so you become familiar with its security features.</span></span>

### <a name="the-starter-app"></a><span data-ttu-id="2d9b7-333">La aplicación de inicio</span><span class="sxs-lookup"><span data-stu-id="2d9b7-333">The starter app</span></span>

<span data-ttu-id="2d9b7-334">[Descargar](xref:index#how-to-download-a-sample) el [starter](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/) app.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-334">[Download](xref:index#how-to-download-a-sample) the [starter](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/) app.</span></span>

<span data-ttu-id="2d9b7-335">Ejecute la aplicación, pulse el **ContactManager** vincular y compruebe que puede crear, editar y eliminar un contacto.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-335">Run the app, tap the **ContactManager** link, and verify you can create, edit, and delete a contact.</span></span>

## <a name="secure-user-data"></a><span data-ttu-id="2d9b7-336">Proteger los datos de usuario</span><span class="sxs-lookup"><span data-stu-id="2d9b7-336">Secure user data</span></span>

<span data-ttu-id="2d9b7-337">Las siguientes secciones contienen todos los pasos principales para crear la aplicación de datos de usuario segura.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-337">The following sections have all the major steps to create the secure user data app.</span></span> <span data-ttu-id="2d9b7-338">Resultarle útil consultar el proyecto completado.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-338">You may find it helpful to refer to the completed project.</span></span>

### <a name="tie-the-contact-data-to-the-user"></a><span data-ttu-id="2d9b7-339">Vincular los datos de contacto para el usuario</span><span class="sxs-lookup"><span data-stu-id="2d9b7-339">Tie the contact data to the user</span></span>

<span data-ttu-id="2d9b7-340">Usar ASP.NET [identidad](xref:security/authentication/identity) Id. de usuario para garantizar que los usuarios puede editar sus datos, pero no otros datos de los usuarios.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-340">Use the ASP.NET [Identity](xref:security/authentication/identity) user ID to ensure users can edit their data, but not other users data.</span></span> <span data-ttu-id="2d9b7-341">Agregar `OwnerID` y `ContactStatus` a la `Contact` modelo:</span><span class="sxs-lookup"><span data-stu-id="2d9b7-341">Add `OwnerID` and `ContactStatus` to the `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Models/Contact.cs?name=snippet1&highlight=5-6,16-999)]

<span data-ttu-id="2d9b7-342">`OwnerID` es el identificador del usuario desde el `AspNetUser` de tabla en la [identidad](xref:security/authentication/identity) base de datos.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-342">`OwnerID` is the user's ID from the `AspNetUser` table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="2d9b7-343">El `Status` campo determina si un contacto es visible para los usuarios en general.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-343">The `Status` field determines if a contact is viewable by general users.</span></span>

<span data-ttu-id="2d9b7-344">Crear una nueva migración y actualización de la base de datos:</span><span class="sxs-lookup"><span data-stu-id="2d9b7-344">Create a new migration and update the database:</span></span>

```console
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="add-role-services-to-identity"></a><span data-ttu-id="2d9b7-345">Agregar servicios de función a la identidad</span><span class="sxs-lookup"><span data-stu-id="2d9b7-345">Add Role services to Identity</span></span>

<span data-ttu-id="2d9b7-346">Anexar [AddRoles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) para agregar servicios de rol:</span><span class="sxs-lookup"><span data-stu-id="2d9b7-346">Append [AddRoles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) to add Role services:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet2&highlight=12)]

### <a name="require-authenticated-users"></a><span data-ttu-id="2d9b7-347">Requerir que los usuarios autenticados</span><span class="sxs-lookup"><span data-stu-id="2d9b7-347">Require authenticated users</span></span>

<span data-ttu-id="2d9b7-348">Establezca la directiva de autenticación predeterminado para exigir que los usuarios se autentiquen:</span><span class="sxs-lookup"><span data-stu-id="2d9b7-348">Set the default authentication policy to require users to be authenticated:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet&highlight=17-99)] 

 <span data-ttu-id="2d9b7-349">Puede rechazar la autenticación en el nivel de método de página de Razor, controlador o acción con el `[AllowAnonymous]` atributo.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-349">You can opt out of authentication at the Razor Page, controller, or action method level with the `[AllowAnonymous]` attribute.</span></span> <span data-ttu-id="2d9b7-350">Configuración de la directiva de autenticación predeterminado para exigir que los usuarios se autentiquen protege recién agregada de las páginas de Razor y controladores.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-350">Setting the default authentication policy to require users to be authenticated protects newly added Razor Pages and controllers.</span></span> <span data-ttu-id="2d9b7-351">Tener la autenticación requerida por el valor predeterminado es más segura que confiar en los nuevos controladores y las páginas de Razor debe incluir el `[Authorize]` atributo.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-351">Having authentication required by default is more secure than relying on new controllers and Razor Pages to include the `[Authorize]` attribute.</span></span>

<span data-ttu-id="2d9b7-352">Agregar [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) al índice, acerca de y póngase en contacto con páginas para los usuarios anónimos pueden obtener información sobre el sitio antes de que se registren.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-352">Add [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) to the Index, About, and Contact pages so anonymous users can get information about the site before they register.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Index.cshtml.cs?highlight=1,6)]

### <a name="configure-the-test-account"></a><span data-ttu-id="2d9b7-353">Configurar la cuenta de prueba</span><span class="sxs-lookup"><span data-stu-id="2d9b7-353">Configure the test account</span></span>

<span data-ttu-id="2d9b7-354">La `SeedData` clase crea dos cuentas: administrador y administrador.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-354">The `SeedData` class creates two accounts: administrator and manager.</span></span> <span data-ttu-id="2d9b7-355">Use la [herramienta Secret Manager](xref:security/app-secrets) para establecer una contraseña para estas cuentas.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-355">Use the [Secret Manager tool](xref:security/app-secrets) to set a password for these accounts.</span></span> <span data-ttu-id="2d9b7-356">Establecer la contraseña del directorio de proyecto (el directorio que contiene *Program.cs*):</span><span class="sxs-lookup"><span data-stu-id="2d9b7-356">Set the password from the project directory (the directory containing *Program.cs*):</span></span>

```console
dotnet user-secrets set SeedUserPW <PW>
```

<span data-ttu-id="2d9b7-357">Si no se especifica una contraseña segura, se produce una excepción cuando `SeedData.Initialize` se llama.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-357">If a strong password is not specified, an exception is thrown when `SeedData.Initialize` is called.</span></span>

<span data-ttu-id="2d9b7-358">Actualización `Main` usar la contraseña de la prueba:</span><span class="sxs-lookup"><span data-stu-id="2d9b7-358">Update `Main` to use the test password:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a><span data-ttu-id="2d9b7-359">Crear las cuentas de prueba y actualizar los contactos</span><span class="sxs-lookup"><span data-stu-id="2d9b7-359">Create the test accounts and update the contacts</span></span>

<span data-ttu-id="2d9b7-360">Actualización de la `Initialize` método en el `SeedData` clase para crear las cuentas de prueba:</span><span class="sxs-lookup"><span data-stu-id="2d9b7-360">Update the `Initialize` method in the `SeedData` class to create the test accounts:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Data/SeedData.cs?name=snippet_Initialize)]

<span data-ttu-id="2d9b7-361">Agregue el identificador de usuario administrador y `ContactStatus` a los contactos.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-361">Add the administrator user ID and `ContactStatus` to the contacts.</span></span> <span data-ttu-id="2d9b7-362">Realice uno de los contactos "Enviado" y un "rechazada".</span><span class="sxs-lookup"><span data-stu-id="2d9b7-362">Make one of the contacts "Submitted" and one "Rejected".</span></span> <span data-ttu-id="2d9b7-363">Agregue el Id. de usuario y el estado a todos los contactos.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-363">Add the user ID and status to all the contacts.</span></span> <span data-ttu-id="2d9b7-364">Póngase en contacto un solo con se muestra:</span><span class="sxs-lookup"><span data-stu-id="2d9b7-364">Only one contact is shown:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a><span data-ttu-id="2d9b7-365">Crear controladores de autorización de administrador, administrador y propietario</span><span class="sxs-lookup"><span data-stu-id="2d9b7-365">Create owner, manager, and administrator authorization handlers</span></span>

<span data-ttu-id="2d9b7-366">Crear un `ContactIsOwnerAuthorizationHandler` clase en el *autorización* carpeta.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-366">Create a `ContactIsOwnerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="2d9b7-367">El `ContactIsOwnerAuthorizationHandler` comprueba que el usuario que actúan en un recurso posee el recurso.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-367">The `ContactIsOwnerAuthorizationHandler` verifies that the user acting on a resource owns the resource.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

<span data-ttu-id="2d9b7-368">El `ContactIsOwnerAuthorizationHandler` llamadas [contexto. Se realizan correctamente](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) si el usuario autenticado actual es el propietario del contacto.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-368">The `ContactIsOwnerAuthorizationHandler` calls [context.Succeed](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) if the current authenticated user is the contact owner.</span></span> <span data-ttu-id="2d9b7-369">Controladores de autorización con carácter general:</span><span class="sxs-lookup"><span data-stu-id="2d9b7-369">Authorization handlers generally:</span></span>

* <span data-ttu-id="2d9b7-370">Devolver `context.Succeed` cuando se cumplen los requisitos.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-370">Return `context.Succeed` when the requirements are met.</span></span>
* <span data-ttu-id="2d9b7-371">Devolver `Task.CompletedTask` cuando no se cumplen los requisitos.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-371">Return `Task.CompletedTask` when requirements aren't met.</span></span> <span data-ttu-id="2d9b7-372">`Task.CompletedTask`no es correcto o erróneo&mdash;permite que se ejecuten otros controladores de autorización.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-372">`Task.CompletedTask` is not success or failure&mdash;it allows other authorization handlers to run.</span></span>

<span data-ttu-id="2d9b7-373">Si necesita explícitamente un error, devolver [contexto. Un error](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span><span class="sxs-lookup"><span data-stu-id="2d9b7-373">If you need to explicitly fail, return [context.Fail](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span></span>

<span data-ttu-id="2d9b7-374">La aplicación permite a los propietarios de contacto para editar, eliminar o crear sus propios datos.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-374">The app allows contact owners to edit/delete/create their own data.</span></span> <span data-ttu-id="2d9b7-375">`ContactIsOwnerAuthorizationHandler` no es necesario comprobar la operación que se pasa en el parámetro de requisito.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-375">`ContactIsOwnerAuthorizationHandler` doesn't need to check the operation passed in the requirement parameter.</span></span>

### <a name="create-a-manager-authorization-handler"></a><span data-ttu-id="2d9b7-376">Crear un controlador de autorización de administrador</span><span class="sxs-lookup"><span data-stu-id="2d9b7-376">Create a manager authorization handler</span></span>

<span data-ttu-id="2d9b7-377">Crear un `ContactManagerAuthorizationHandler` clase en el *autorización* carpeta.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-377">Create a `ContactManagerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="2d9b7-378">El `ContactManagerAuthorizationHandler` comprueba que el usuario que actúan en el recurso es un administrador.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-378">The `ContactManagerAuthorizationHandler` verifies the user acting on the resource is a manager.</span></span> <span data-ttu-id="2d9b7-379">Solo los administradores pueden aprobar o rechazar los cambios de contenido (nuevos o modificados).</span><span class="sxs-lookup"><span data-stu-id="2d9b7-379">Only managers can approve or reject content changes (new or changed).</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a><span data-ttu-id="2d9b7-380">Cree un controlador de autorización de administrador</span><span class="sxs-lookup"><span data-stu-id="2d9b7-380">Create an administrator authorization handler</span></span>

<span data-ttu-id="2d9b7-381">Crear un `ContactAdministratorsAuthorizationHandler` clase en el *autorización* carpeta.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-381">Create a `ContactAdministratorsAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="2d9b7-382">El `ContactAdministratorsAuthorizationHandler` comprueba que el usuario que actúan en el recurso es un administrador.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-382">The `ContactAdministratorsAuthorizationHandler` verifies the user acting on the resource is an administrator.</span></span> <span data-ttu-id="2d9b7-383">Administrador puede realizar todas las operaciones.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-383">Administrator can do all operations.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a><span data-ttu-id="2d9b7-384">Registrar los controladores de autorización</span><span class="sxs-lookup"><span data-stu-id="2d9b7-384">Register the authorization handlers</span></span>

<span data-ttu-id="2d9b7-385">Se deben registrar los servicios mediante Entity Framework Core para [inserción de dependencias](xref:fundamentals/dependency-injection) mediante [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span><span class="sxs-lookup"><span data-stu-id="2d9b7-385">Services using Entity Framework Core must be registered for [dependency injection](xref:fundamentals/dependency-injection) using [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span></span> <span data-ttu-id="2d9b7-386">El `ContactIsOwnerAuthorizationHandler` utiliza ASP.NET Core [identidad](xref:security/authentication/identity), que se basa en Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-386">The `ContactIsOwnerAuthorizationHandler` uses ASP.NET Core [Identity](xref:security/authentication/identity), which is built on Entity Framework Core.</span></span> <span data-ttu-id="2d9b7-387">Registre los controladores con la colección de servicios para que estén disponibles para el `ContactsController` a través de [inserción de dependencias](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="2d9b7-387">Register the handlers with the service collection so they're available to the `ContactsController` through [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="2d9b7-388">Agregue el código siguiente al final de `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="2d9b7-388">Add the following code to the end of `ConfigureServices`:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet_defaultPolicy&highlight=27-99)]

<span data-ttu-id="2d9b7-389">`ContactAdministratorsAuthorizationHandler` y `ContactManagerAuthorizationHandler` se agregan como singleton.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-389">`ContactAdministratorsAuthorizationHandler` and `ContactManagerAuthorizationHandler` are added as singletons.</span></span> <span data-ttu-id="2d9b7-390">Lo singletons con estado porque no usan EF y toda la información necesaria se encuentra en la `Context` parámetro de la `HandleRequirementAsync` método.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-390">They're singletons because they don't use EF and all the information needed is in the `Context` parameter of the `HandleRequirementAsync` method.</span></span>

## <a name="support-authorization"></a><span data-ttu-id="2d9b7-391">Admitir la autorización</span><span class="sxs-lookup"><span data-stu-id="2d9b7-391">Support authorization</span></span>

<span data-ttu-id="2d9b7-392">En esta sección, actualice las páginas de Razor y agregue una clase de los requisitos de operaciones.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-392">In this section, you update the Razor Pages and add an operations requirements class.</span></span>

### <a name="review-the-contact-operations-requirements-class"></a><span data-ttu-id="2d9b7-393">Revisión de la clase de los requisitos de las operaciones de contacto</span><span class="sxs-lookup"><span data-stu-id="2d9b7-393">Review the contact operations requirements class</span></span>

<span data-ttu-id="2d9b7-394">Revise el `ContactOperations` clase.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-394">Review the `ContactOperations` class.</span></span> <span data-ttu-id="2d9b7-395">Esta clase contiene los requisitos que admite la aplicación:</span><span class="sxs-lookup"><span data-stu-id="2d9b7-395">This class contains the requirements the app supports:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-contacts-razor-pages"></a><span data-ttu-id="2d9b7-396">Crear una clase base para las páginas de Razor de contactos</span><span class="sxs-lookup"><span data-stu-id="2d9b7-396">Create a base class for the Contacts Razor Pages</span></span>

<span data-ttu-id="2d9b7-397">Cree una clase base que contiene los servicios usados en los contactos de las páginas de Razor.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-397">Create a base class that contains the services used in the contacts Razor Pages.</span></span> <span data-ttu-id="2d9b7-398">La clase base coloca el código de inicialización en una ubicación:</span><span class="sxs-lookup"><span data-stu-id="2d9b7-398">The base class puts the initialization code in one location:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/DI_BasePageModel.cs)]

<span data-ttu-id="2d9b7-399">El código anterior:</span><span class="sxs-lookup"><span data-stu-id="2d9b7-399">The preceding code:</span></span>

* <span data-ttu-id="2d9b7-400">Agrega el `IAuthorizationService` service acceda a los controladores de autorización.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-400">Adds the `IAuthorizationService` service to access to the authorization handlers.</span></span>
* <span data-ttu-id="2d9b7-401">Agrega la identidad `UserManager` service.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-401">Adds the Identity `UserManager` service.</span></span>
* <span data-ttu-id="2d9b7-402">Agregue la `ApplicationDbContext`.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-402">Add the `ApplicationDbContext`.</span></span>

### <a name="update-the-createmodel"></a><span data-ttu-id="2d9b7-403">Actualizar el CreateModel</span><span class="sxs-lookup"><span data-stu-id="2d9b7-403">Update the CreateModel</span></span>

<span data-ttu-id="2d9b7-404">Actualizar el constructor del modelo de página create para usar el `DI_BasePageModel` clase base:</span><span class="sxs-lookup"><span data-stu-id="2d9b7-404">Update the create page model constructor to use the `DI_BasePageModel` base class:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

<span data-ttu-id="2d9b7-405">Actualización de la `CreateModel.OnPostAsync` método:</span><span class="sxs-lookup"><span data-stu-id="2d9b7-405">Update the `CreateModel.OnPostAsync` method to:</span></span>

* <span data-ttu-id="2d9b7-406">Agregue el identificador de usuario para el `Contact` modelo.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-406">Add the user ID to the `Contact` model.</span></span>
* <span data-ttu-id="2d9b7-407">Llamar al controlador de autorización para comprobar que el usuario tiene permiso para crear contactos.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-407">Call the authorization handler to verify the user has permission to create contacts.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a><span data-ttu-id="2d9b7-408">Actualizar el IndexModel</span><span class="sxs-lookup"><span data-stu-id="2d9b7-408">Update the IndexModel</span></span>

<span data-ttu-id="2d9b7-409">Actualización de la `OnGetAsync` método por lo que solo contactos aprobados se muestran a los usuarios generales:</span><span class="sxs-lookup"><span data-stu-id="2d9b7-409">Update the `OnGetAsync` method so only approved contacts are shown to general users:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a><span data-ttu-id="2d9b7-410">Actualizar el EditModel</span><span class="sxs-lookup"><span data-stu-id="2d9b7-410">Update the EditModel</span></span>

<span data-ttu-id="2d9b7-411">Agregar un controlador de autorización para comprobar que el usuario propietario del contacto.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-411">Add an authorization handler to verify the user owns the contact.</span></span> <span data-ttu-id="2d9b7-412">Dado que se está validando la autorización de recursos, el `[Authorize]` atributo no es suficiente.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-412">Because resource authorization is being validated, the `[Authorize]` attribute is not enough.</span></span> <span data-ttu-id="2d9b7-413">La aplicación no tiene acceso al recurso cuando se evalúan los atributos.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-413">The app doesn't have access to the resource when attributes are evaluated.</span></span> <span data-ttu-id="2d9b7-414">Autorización basada en el recurso debe ser un imperativo.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-414">Resource-based authorization must be imperative.</span></span> <span data-ttu-id="2d9b7-415">Las comprobaciones deben realizarse una vez que la aplicación tenga acceso al recurso, cargándolo en el modelo de página o cargándola en el controlador de sí mismo.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-415">Checks must be performed once the app has access to the resource, either by loading it in the page model or by loading it within the handler itself.</span></span> <span data-ttu-id="2d9b7-416">Con frecuencia acceder al recurso pasando la clave de recurso.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-416">You frequently access the resource by passing in the resource key.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a><span data-ttu-id="2d9b7-417">Actualizar el DeleteModel</span><span class="sxs-lookup"><span data-stu-id="2d9b7-417">Update the DeleteModel</span></span>

<span data-ttu-id="2d9b7-418">Actualice el modelo de página de delete para usar el controlador de autorización para comprobar que el usuario tiene permiso delete en el contacto.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-418">Update the delete page model to use the authorization handler to verify the user has delete permission on the contact.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a><span data-ttu-id="2d9b7-419">Insertar el servicio de autorización en las vistas</span><span class="sxs-lookup"><span data-stu-id="2d9b7-419">Inject the authorization service into the views</span></span>

<span data-ttu-id="2d9b7-420">Actualmente, se muestra en la interfaz de usuario edita y elimina los vínculos para los contactos que no se puede modificar el usuario.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-420">Currently, the UI shows edit and delete links for contacts the user can't modify.</span></span>

<span data-ttu-id="2d9b7-421">Insertar el servicio de autorización en el *Views/_viewimports.cshtml* para que esté disponible para todas las vistas de archivos:</span><span class="sxs-lookup"><span data-stu-id="2d9b7-421">Inject the authorization service in the *Views/_ViewImports.cshtml* file so it's available to all views:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/_ViewImports.cshtml?highlight=6-99)]

<span data-ttu-id="2d9b7-422">El marcado anterior agrega varios `using` instrucciones.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-422">The preceding markup adds several `using` statements.</span></span>

<span data-ttu-id="2d9b7-423">Actualización de la **editar** y **eliminar** vincula en *Pages/Contacts/Index.cshtml* por lo que sólo se representen a los usuarios con los permisos adecuados:</span><span class="sxs-lookup"><span data-stu-id="2d9b7-423">Update the **Edit** and **Delete** links in *Pages/Contacts/Index.cshtml* so they're only rendered for users with the appropriate permissions:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Index.cshtml?highlight=34-36,62-999)]

> [!WARNING]
> <span data-ttu-id="2d9b7-424">Ocultar los vínculos de los usuarios que no tienen permiso para cambiar los datos no proteger la aplicación.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-424">Hiding links from users that don't have permission to change data doesn't secure the app.</span></span> <span data-ttu-id="2d9b7-425">Ocultar vínculos hace que la aplicación más fácil de usar mostrando vínculos solo es válido.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-425">Hiding links makes the app more user-friendly by displaying only valid links.</span></span> <span data-ttu-id="2d9b7-426">Los usuarios pueden hack las direcciones URL generadas para invocar Editar y eliminar operaciones en los datos que no poseen.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-426">Users can hack the generated URLs to invoke edit and delete operations on data they don't own.</span></span> <span data-ttu-id="2d9b7-427">La página de Razor o el controlador debe exigir comprobaciones de acceso para proteger los datos.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-427">The Razor Page or controller must enforce access checks to secure the data.</span></span>

### <a name="update-details"></a><span data-ttu-id="2d9b7-428">Detalles de la actualización</span><span class="sxs-lookup"><span data-stu-id="2d9b7-428">Update Details</span></span>

<span data-ttu-id="2d9b7-429">Actualice la vista de detalles para que los administradores pueden aprobar o rechazar contactos:</span><span class="sxs-lookup"><span data-stu-id="2d9b7-429">Update the details view so managers can approve or reject contacts:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml?name=snippet)]

<span data-ttu-id="2d9b7-430">Actualice el modelo de página de detalles:</span><span class="sxs-lookup"><span data-stu-id="2d9b7-430">Update the details page model:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="add-or-remove-a-user-to-a-role"></a><span data-ttu-id="2d9b7-431">Agregar o quitar un usuario a un rol</span><span class="sxs-lookup"><span data-stu-id="2d9b7-431">Add or remove a user to a role</span></span>

<span data-ttu-id="2d9b7-432">Consulte [este problema](https://github.com/aspnet/AspNetCore.Docs/issues/8502) para obtener información sobre:</span><span class="sxs-lookup"><span data-stu-id="2d9b7-432">See [this issue](https://github.com/aspnet/AspNetCore.Docs/issues/8502) for information on:</span></span>

* <span data-ttu-id="2d9b7-433">Quita los privilegios de un usuario.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-433">Removing privileges from a user.</span></span> <span data-ttu-id="2d9b7-434">Por ejemplo, silenciar a un usuario en una aplicación de chat.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-434">For example, muting a user in a chat app.</span></span>
* <span data-ttu-id="2d9b7-435">Agregar privilegios a un usuario.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-435">Adding privileges to a user.</span></span>

## <a name="test-the-completed-app"></a><span data-ttu-id="2d9b7-436">Probar la aplicación completada</span><span class="sxs-lookup"><span data-stu-id="2d9b7-436">Test the completed app</span></span>

<span data-ttu-id="2d9b7-437">Si ya no ha establecido una contraseña para cuentas de usuario inicializados, utilice el [herramienta Secret Manager](xref:security/app-secrets#secret-manager) para establecer una contraseña:</span><span class="sxs-lookup"><span data-stu-id="2d9b7-437">If you haven't already set a password for seeded user accounts, use the [Secret Manager tool](xref:security/app-secrets#secret-manager) to set a password:</span></span>

* <span data-ttu-id="2d9b7-438">Elija una contraseña segura: Use ocho o más caracteres y al menos un carácter, número y símbolo en mayúsculas.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-438">Choose a strong password: Use eight or more characters and at least one upper-case character, number, and symbol.</span></span> <span data-ttu-id="2d9b7-439">Por ejemplo, `Passw0rd!` cumple los requisitos de contraseña segura.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-439">For example, `Passw0rd!` meets the strong password requirements.</span></span>
* <span data-ttu-id="2d9b7-440">Ejecute el siguiente comando desde la carpeta del proyecto, donde `<PW>` es la contraseña:</span><span class="sxs-lookup"><span data-stu-id="2d9b7-440">Execute the following command from the project's folder, where `<PW>` is the password:</span></span>

  ```console
  dotnet user-secrets set SeedUserPW <PW>
  ```

* <span data-ttu-id="2d9b7-441">Quitar y actualizar la base de datos</span><span class="sxs-lookup"><span data-stu-id="2d9b7-441">Drop and update the Database</span></span>

    ```console
     dotnet ef database drop -f
     dotnet ef database update  
     ```

* <span data-ttu-id="2d9b7-442">Reinicie la aplicación para inicializar la base de datos.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-442">Restart the app to seed the database.</span></span>

<span data-ttu-id="2d9b7-443">Es una manera fácil de probar la aplicación completa iniciar los tres distintos exploradores (o las sesiones de InPrivate o incognito).</span><span class="sxs-lookup"><span data-stu-id="2d9b7-443">An easy way to test the completed app is to launch three different browsers (or incognito/InPrivate sessions).</span></span> <span data-ttu-id="2d9b7-444">En un explorador, registre un nuevo usuario (por ejemplo, `test@example.com`).</span><span class="sxs-lookup"><span data-stu-id="2d9b7-444">In one browser, register a new user (for example, `test@example.com`).</span></span> <span data-ttu-id="2d9b7-445">Inicie sesión con un usuario diferente en cada explorador.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-445">Sign in to each browser with a different user.</span></span> <span data-ttu-id="2d9b7-446">Compruebe las siguientes operaciones:</span><span class="sxs-lookup"><span data-stu-id="2d9b7-446">Verify the following operations:</span></span>

* <span data-ttu-id="2d9b7-447">Usuarios registrados pueden ver todos los datos de contacto aprobados.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-447">Registered users can view all of the approved contact data.</span></span>
* <span data-ttu-id="2d9b7-448">Los usuarios registrados pueden editar o eliminar sus propios datos.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-448">Registered users can edit/delete their own data.</span></span>
* <span data-ttu-id="2d9b7-449">Los administradores pueden aprobar o rechazar datos de contacto.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-449">Managers can approve/reject contact data.</span></span> <span data-ttu-id="2d9b7-450">El `Details` ver muestra **aprobar** y **rechazar** botones.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-450">The `Details` view shows **Approve** and **Reject** buttons.</span></span>
* <span data-ttu-id="2d9b7-451">Los administradores pueden aprobar o rechazar y editar o eliminar todos los datos.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-451">Administrators can approve/reject and edit/delete all data.</span></span>

| <span data-ttu-id="2d9b7-452">Usuario</span><span class="sxs-lookup"><span data-stu-id="2d9b7-452">User</span></span>                | <span data-ttu-id="2d9b7-453">Propagadas por la aplicación</span><span class="sxs-lookup"><span data-stu-id="2d9b7-453">Seeded by the app</span></span> | <span data-ttu-id="2d9b7-454">Opciones</span><span class="sxs-lookup"><span data-stu-id="2d9b7-454">Options</span></span>                                  |
| ------------------- | :---------------: | ---------------------------------------- |
| test@example.com    | <span data-ttu-id="2d9b7-455">No</span><span class="sxs-lookup"><span data-stu-id="2d9b7-455">No</span></span>                | <span data-ttu-id="2d9b7-456">Editar o eliminar los datos propios.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-456">Edit/delete the own data.</span></span>                |
| manager@contoso.com | <span data-ttu-id="2d9b7-457">Sí</span><span class="sxs-lookup"><span data-stu-id="2d9b7-457">Yes</span></span>               | <span data-ttu-id="2d9b7-458">Aprobar o rechazar y editar o eliminar los datos propios.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-458">Approve/reject and edit/delete own data.</span></span> |
| admin@contoso.com   | <span data-ttu-id="2d9b7-459">Sí</span><span class="sxs-lookup"><span data-stu-id="2d9b7-459">Yes</span></span>               | <span data-ttu-id="2d9b7-460">Aprobar o rechazar y editar o eliminar todos los datos.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-460">Approve/reject and edit/delete all data.</span></span> |

<span data-ttu-id="2d9b7-461">Crear un contacto en el explorador del administrador.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-461">Create a contact in the administrator's browser.</span></span> <span data-ttu-id="2d9b7-462">Copie la dirección URL para su eliminación y editar en el contacto del administrador.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-462">Copy the URL for delete and edit from the administrator contact.</span></span> <span data-ttu-id="2d9b7-463">Pegue estos vínculos en el explorador del usuario de prueba para comprobar que el usuario de prueba no puede realizar estas operaciones.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-463">Paste these links into the test user's browser to verify the test user can't perform these operations.</span></span>

## <a name="create-the-starter-app"></a><span data-ttu-id="2d9b7-464">Crear la aplicación de inicio</span><span class="sxs-lookup"><span data-stu-id="2d9b7-464">Create the starter app</span></span>

* <span data-ttu-id="2d9b7-465">Cree una aplicación de páginas de Razor denominada "ContactManager"</span><span class="sxs-lookup"><span data-stu-id="2d9b7-465">Create a Razor Pages app named "ContactManager"</span></span>
  * <span data-ttu-id="2d9b7-466">Creación de la aplicación con **cuentas de usuario individuales**.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-466">Create the app with **Individual User Accounts**.</span></span>
  * <span data-ttu-id="2d9b7-467">Asígnele el nombre "ContactManager" para el espacio de nombres coincida con el espacio de nombres utilizado en el ejemplo.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-467">Name it "ContactManager" so the namespace matches the namespace used in the sample.</span></span>
  * <span data-ttu-id="2d9b7-468">`-uld` Especifica LocalDB en lugar de SQLite</span><span class="sxs-lookup"><span data-stu-id="2d9b7-468">`-uld` specifies LocalDB instead of SQLite</span></span>

  ```console
  dotnet new webapp -o ContactManager -au Individual -uld
  ```

* <span data-ttu-id="2d9b7-469">Agregue *Models/contact. CS*:</span><span class="sxs-lookup"><span data-stu-id="2d9b7-469">Add *Models/Contact.cs*:</span></span>

  [!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

* <span data-ttu-id="2d9b7-470">Scaffold el `Contact` modelo.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-470">Scaffold the `Contact` model.</span></span>
* <span data-ttu-id="2d9b7-471">Crear la migración inicial y actualizar la base de datos:</span><span class="sxs-lookup"><span data-stu-id="2d9b7-471">Create initial migration and update the database:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
  dotnet ef database drop -f
  dotnet ef migrations add initial
  dotnet ef database update
  ```

* <span data-ttu-id="2d9b7-472">Actualización de la **ContactManager** anclar en el *Pages/_Layout.cshtml* archivo:</span><span class="sxs-lookup"><span data-stu-id="2d9b7-472">Update the **ContactManager** anchor in the *Pages/_Layout.cshtml* file:</span></span>

  ```cshtml
  <a asp-page="/Contacts/Index" class="navbar-brand">ContactManager</a>
  ```

* <span data-ttu-id="2d9b7-473">Probar la aplicación mediante la creación, edición y eliminación de un contacto</span><span class="sxs-lookup"><span data-stu-id="2d9b7-473">Test the app by creating, editing, and deleting a contact</span></span>

### <a name="seed-the-database"></a><span data-ttu-id="2d9b7-474">Inicializar la base de datos</span><span class="sxs-lookup"><span data-stu-id="2d9b7-474">Seed the database</span></span>

<span data-ttu-id="2d9b7-475">Agregar el [SeedData](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2.1/Data/SeedData.cs) clase a la *datos* carpeta.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-475">Add the [SeedData](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2.1/Data/SeedData.cs) class to the *Data* folder.</span></span>

<span data-ttu-id="2d9b7-476">Llame a `SeedData.Initialize` desde `Main`:</span><span class="sxs-lookup"><span data-stu-id="2d9b7-476">Call `SeedData.Initialize` from `Main`:</span></span>

[!code-csharp[](secure-data/samples/starter2.1/Program.cs?name=snippet)]

<span data-ttu-id="2d9b7-477">Probar que la aplicación había propagado la base de datos.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-477">Test that the app seeded the database.</span></span> <span data-ttu-id="2d9b7-478">Si no hay ninguna fila en la base de datos de contacto, no se ejecuta el método de inicialización.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-478">If there are any rows in the contact DB, the seed method doesn't run.</span></span>

::: moniker-end

<a name="secure-data-add-resources-label"></a>

### <a name="additional-resources"></a><span data-ttu-id="2d9b7-479">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="2d9b7-479">Additional resources</span></span>

* [<span data-ttu-id="2d9b7-480">Compilar una aplicación web de .NET Core y SQL Database en Azure App Service</span><span class="sxs-lookup"><span data-stu-id="2d9b7-480">Build a .NET Core and SQL Database web app in Azure App Service</span></span>](/azure/app-service/app-service-web-tutorial-dotnetcore-sqldb)
* <span data-ttu-id="2d9b7-481">[Laboratorio de autorización de ASP.NET Core](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span><span class="sxs-lookup"><span data-stu-id="2d9b7-481">[ASP.NET Core Authorization Lab](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span></span> <span data-ttu-id="2d9b7-482">Este laboratorio explica con más detalle en las características de seguridad, presentadas en este tutorial.</span><span class="sxs-lookup"><span data-stu-id="2d9b7-482">This lab goes into more detail on the security features introduced in this tutorial.</span></span>
* <xref:security/authorization/introduction>
* [<span data-ttu-id="2d9b7-483">Autorización personalizada basada en directivas</span><span class="sxs-lookup"><span data-stu-id="2d9b7-483">Custom policy-based authorization</span></span>](xref:security/authorization/policies)
