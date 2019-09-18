---
title: Crear una aplicación ASP.NET Core con datos de usuario protegidos por autorización
author: rick-anderson
description: Obtenga información sobre cómo crear una aplicación de páginas de Razor con datos de usuario protegidos por autorización. Incluye HTTPS, autenticación, seguridad, ASP.NET Core Identity.
ms.author: riande
ms.date: 12/18/2018
ms.custom: mvc, seodec18
uid: security/authorization/secure-data
ms.openlocfilehash: d827f6f839c9e42e6d3d7b04fe8b24a1c9732aee
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2019
ms.locfileid: "71082438"
---
# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a><span data-ttu-id="21c5d-104">Crear una aplicación ASP.NET Core con datos de usuario protegidos por autorización</span><span class="sxs-lookup"><span data-stu-id="21c5d-104">Create an ASP.NET Core app with user data protected by authorization</span></span>

<span data-ttu-id="21c5d-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT) y [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="21c5d-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="21c5d-106">Consulte [este PDF](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) para la versión de ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="21c5d-106">See [this PDF](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core MVC version.</span></span> <span data-ttu-id="21c5d-107">La versión 1.1 de ASP.NET Core de este tutorial se encuentra en [esto](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data) carpeta.</span><span class="sxs-lookup"><span data-stu-id="21c5d-107">The ASP.NET Core 1.1 version of this tutorial is in [this](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data) folder.</span></span> <span data-ttu-id="21c5d-108">La 1.1 de ejemplo de ASP.NET Core se encuentra en la [ejemplos](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).</span><span class="sxs-lookup"><span data-stu-id="21c5d-108">The 1.1 ASP.NET Core sample is in the [samples](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="21c5d-109">Consulte [este pdf](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_July16_18.pdf)</span><span class="sxs-lookup"><span data-stu-id="21c5d-109">See [this pdf](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_July16_18.pdf)</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="21c5d-110">Este tutorial muestra cómo crear una aplicación web ASP.NET Core con los datos protegidos por autorización del usuario.</span><span class="sxs-lookup"><span data-stu-id="21c5d-110">This tutorial shows how to create an ASP.NET Core web app with user data protected by authorization.</span></span> <span data-ttu-id="21c5d-111">Muestra una lista de contactos que autentican los usuarios (registrados) ha creado.</span><span class="sxs-lookup"><span data-stu-id="21c5d-111">It displays a list of contacts that authenticated (registered) users have created.</span></span> <span data-ttu-id="21c5d-112">Hay tres grupos de seguridad:</span><span class="sxs-lookup"><span data-stu-id="21c5d-112">There are three security groups:</span></span>

* <span data-ttu-id="21c5d-113">**Usuarios registrados** puede ver todos los datos aprobados y pueden sus propios datos de editar o eliminar.</span><span class="sxs-lookup"><span data-stu-id="21c5d-113">**Registered users** can view all the approved data and can edit/delete their own data.</span></span>
* <span data-ttu-id="21c5d-114">**Los administradores de** puede aprobar o rechazar los datos de contacto.</span><span class="sxs-lookup"><span data-stu-id="21c5d-114">**Managers** can approve or reject contact data.</span></span> <span data-ttu-id="21c5d-115">Solo contactos aprobados son visibles para los usuarios.</span><span class="sxs-lookup"><span data-stu-id="21c5d-115">Only approved contacts are visible to users.</span></span>
* <span data-ttu-id="21c5d-116">**Los administradores** puede aprobar o rechazar y editar o eliminar todos los datos.</span><span class="sxs-lookup"><span data-stu-id="21c5d-116">**Administrators** can approve/reject and edit/delete any data.</span></span>

<span data-ttu-id="21c5d-117">Las imágenes de este documento no coinciden exactamente con las plantillas más recientes.</span><span class="sxs-lookup"><span data-stu-id="21c5d-117">The images in this document don't exactly match the latest templates.</span></span>

<span data-ttu-id="21c5d-118">En la siguiente imagen, el usuario Rick (`rick@example.com`) ha iniciado sesión.</span><span class="sxs-lookup"><span data-stu-id="21c5d-118">In the following image, user Rick (`rick@example.com`) is signed in.</span></span> <span data-ttu-id="21c5d-119">Rick solo puede ver los contactos aprobados y **editar**/**eliminar**/**crear nuevo** vínculos para sus contactos.</span><span class="sxs-lookup"><span data-stu-id="21c5d-119">Rick can only view approved contacts and **Edit**/**Delete**/**Create New** links for his contacts.</span></span> <span data-ttu-id="21c5d-120">El último registro, creado por Rick, muestra **editar** y **eliminar** vínculos.</span><span class="sxs-lookup"><span data-stu-id="21c5d-120">Only the last record, created by Rick, displays **Edit** and **Delete** links.</span></span> <span data-ttu-id="21c5d-121">Otros usuarios no verán el último registro hasta un administrador o un administrador cambia el estado a "Aprobado".</span><span class="sxs-lookup"><span data-stu-id="21c5d-121">Other users won't see the last record until a manager or administrator changes the status to "Approved".</span></span>

![Captura de pantalla con Rick ha iniciado sesión](secure-data/_static/rick.png)

<span data-ttu-id="21c5d-123">En la siguiente imagen, `manager@contoso.com` ha iniciado sesión y en el rol del administrador:</span><span class="sxs-lookup"><span data-stu-id="21c5d-123">In the following image, `manager@contoso.com` is signed in and in the manager's role:</span></span>

![Captura de pantalla mostrando manager@contoso.com iniciada](secure-data/_static/manager1.png)

<span data-ttu-id="21c5d-125">La siguiente imagen muestra a los administradores de la vista de detalles de un contacto:</span><span class="sxs-lookup"><span data-stu-id="21c5d-125">The following image shows the managers details view of a contact:</span></span>

![Vista del Administrador de un contacto](secure-data/_static/manager.png)

<span data-ttu-id="21c5d-127">El **aprobar** y **rechazar** solo se muestran los botones para administradores y administradores.</span><span class="sxs-lookup"><span data-stu-id="21c5d-127">The **Approve** and **Reject** buttons are only displayed for managers and administrators.</span></span>

<span data-ttu-id="21c5d-128">En la siguiente imagen, `admin@contoso.com` ha iniciado sesión y en el rol de administrador:</span><span class="sxs-lookup"><span data-stu-id="21c5d-128">In the following image, `admin@contoso.com` is signed in and in the administrator's role:</span></span>

![Captura de pantalla mostrando admin@contoso.com iniciada](secure-data/_static/admin.png)

<span data-ttu-id="21c5d-130">El administrador tiene todos los privilegios.</span><span class="sxs-lookup"><span data-stu-id="21c5d-130">The administrator has all privileges.</span></span> <span data-ttu-id="21c5d-131">Puede leer, editar o eliminar cualquier contacto y cambiar el estado de los contactos.</span><span class="sxs-lookup"><span data-stu-id="21c5d-131">She can read/edit/delete any contact and change the status of contacts.</span></span>

<span data-ttu-id="21c5d-132">Se ha creado la aplicación por [scaffolding](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model) siguiente `Contact` modelo:</span><span class="sxs-lookup"><span data-stu-id="21c5d-132">The app was created by [scaffolding](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model) the following `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

<span data-ttu-id="21c5d-133">El ejemplo contiene los siguientes controladores de autorización:</span><span class="sxs-lookup"><span data-stu-id="21c5d-133">The sample contains the following authorization handlers:</span></span>

* <span data-ttu-id="21c5d-134">`ContactIsOwnerAuthorizationHandler`: Garantiza que un usuario solo puede editar sus datos.</span><span class="sxs-lookup"><span data-stu-id="21c5d-134">`ContactIsOwnerAuthorizationHandler`: Ensures that a user can only edit their data.</span></span>
* <span data-ttu-id="21c5d-135">`ContactManagerAuthorizationHandler`: Permite a los administradores aprobar o rechazar contactos.</span><span class="sxs-lookup"><span data-stu-id="21c5d-135">`ContactManagerAuthorizationHandler`: Allows managers to approve or reject contacts.</span></span>
* <span data-ttu-id="21c5d-136">`ContactAdministratorsAuthorizationHandler`: Permite a los administradores aprobar o rechazar contactos y editar o eliminar contactos.</span><span class="sxs-lookup"><span data-stu-id="21c5d-136">`ContactAdministratorsAuthorizationHandler`: Allows administrators to approve or reject contacts and to edit/delete contacts.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="21c5d-137">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="21c5d-137">Prerequisites</span></span>

<span data-ttu-id="21c5d-138">En este tutorial se avanza.</span><span class="sxs-lookup"><span data-stu-id="21c5d-138">This tutorial is advanced.</span></span> <span data-ttu-id="21c5d-139">Debe estar familiarizado con:</span><span class="sxs-lookup"><span data-stu-id="21c5d-139">You should be familiar with:</span></span>

* [<span data-ttu-id="21c5d-140">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="21c5d-140">ASP.NET Core</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="21c5d-141">Autenticación</span><span class="sxs-lookup"><span data-stu-id="21c5d-141">Authentication</span></span>](xref:security/authentication/identity)
* [<span data-ttu-id="21c5d-142">Confirmación de cuentas y recuperación de contraseñas</span><span class="sxs-lookup"><span data-stu-id="21c5d-142">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="21c5d-143">Autorización</span><span class="sxs-lookup"><span data-stu-id="21c5d-143">Authorization</span></span>](xref:security/authorization/introduction)
* [<span data-ttu-id="21c5d-144">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="21c5d-144">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

## <a name="the-starter-and-completed-app"></a><span data-ttu-id="21c5d-145">El inicio y la aplicación completada</span><span class="sxs-lookup"><span data-stu-id="21c5d-145">The starter and completed app</span></span>

<span data-ttu-id="21c5d-146">[Descargar](xref:index#how-to-download-a-sample) el [completado](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples) app.</span><span class="sxs-lookup"><span data-stu-id="21c5d-146">[Download](xref:index#how-to-download-a-sample) the [completed](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples) app.</span></span> <span data-ttu-id="21c5d-147">[Prueba](#test-the-completed-app) la aplicación completa, por lo que se familiarice con sus características de seguridad.</span><span class="sxs-lookup"><span data-stu-id="21c5d-147">[Test](#test-the-completed-app) the completed app so you become familiar with its security features.</span></span>

### <a name="the-starter-app"></a><span data-ttu-id="21c5d-148">La aplicación de inicio</span><span class="sxs-lookup"><span data-stu-id="21c5d-148">The starter app</span></span>

<span data-ttu-id="21c5d-149">[Descargar](xref:index#how-to-download-a-sample) el [starter](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/) app.</span><span class="sxs-lookup"><span data-stu-id="21c5d-149">[Download](xref:index#how-to-download-a-sample) the [starter](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/) app.</span></span>

<span data-ttu-id="21c5d-150">Ejecute la aplicación, pulse el **ContactManager** vincular y compruebe que puede crear, editar y eliminar un contacto.</span><span class="sxs-lookup"><span data-stu-id="21c5d-150">Run the app, tap the **ContactManager** link, and verify you can create, edit, and delete a contact.</span></span>

## <a name="secure-user-data"></a><span data-ttu-id="21c5d-151">Proteger los datos de usuario</span><span class="sxs-lookup"><span data-stu-id="21c5d-151">Secure user data</span></span>

<span data-ttu-id="21c5d-152">Las siguientes secciones contienen todos los pasos principales para crear la aplicación de datos de usuario segura.</span><span class="sxs-lookup"><span data-stu-id="21c5d-152">The following sections have all the major steps to create the secure user data app.</span></span> <span data-ttu-id="21c5d-153">Resultarle útil consultar el proyecto completado.</span><span class="sxs-lookup"><span data-stu-id="21c5d-153">You may find it helpful to refer to the completed project.</span></span>

### <a name="tie-the-contact-data-to-the-user"></a><span data-ttu-id="21c5d-154">Vincular los datos de contacto para el usuario</span><span class="sxs-lookup"><span data-stu-id="21c5d-154">Tie the contact data to the user</span></span>

<span data-ttu-id="21c5d-155">Usar ASP.NET [identidad](xref:security/authentication/identity) Id. de usuario para garantizar que los usuarios puede editar sus datos, pero no otros datos de los usuarios.</span><span class="sxs-lookup"><span data-stu-id="21c5d-155">Use the ASP.NET [Identity](xref:security/authentication/identity) user ID to ensure users can edit their data, but not other users data.</span></span> <span data-ttu-id="21c5d-156">Agregar `OwnerID` y `ContactStatus` a la `Contact` modelo:</span><span class="sxs-lookup"><span data-stu-id="21c5d-156">Add `OwnerID` and `ContactStatus` to the `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/final3/Models/Contact.cs?name=snippet1&highlight=5-6,16-999)]

<span data-ttu-id="21c5d-157">`OwnerID` es el identificador del usuario desde el `AspNetUser` de tabla en la [identidad](xref:security/authentication/identity) base de datos.</span><span class="sxs-lookup"><span data-stu-id="21c5d-157">`OwnerID` is the user's ID from the `AspNetUser` table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="21c5d-158">El `Status` campo determina si un contacto es visible para los usuarios en general.</span><span class="sxs-lookup"><span data-stu-id="21c5d-158">The `Status` field determines if a contact is viewable by general users.</span></span>

<span data-ttu-id="21c5d-159">Crear una nueva migración y actualización de la base de datos:</span><span class="sxs-lookup"><span data-stu-id="21c5d-159">Create a new migration and update the database:</span></span>

```dotnetcli
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="add-role-services-to-identity"></a><span data-ttu-id="21c5d-160">Agregar servicios de función a la identidad</span><span class="sxs-lookup"><span data-stu-id="21c5d-160">Add Role services to Identity</span></span>

<span data-ttu-id="21c5d-161">Anexar [AddRoles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) para agregar servicios de rol:</span><span class="sxs-lookup"><span data-stu-id="21c5d-161">Append [AddRoles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) to add Role services:</span></span>

[!code-csharp[](secure-data/samples/final3/Startup.cs?name=snippet2&highlight=9)]

### <a name="require-authenticated-users"></a><span data-ttu-id="21c5d-162">Requerir que los usuarios autenticados</span><span class="sxs-lookup"><span data-stu-id="21c5d-162">Require authenticated users</span></span>

<span data-ttu-id="21c5d-163">Establezca la directiva de autenticación predeterminado para exigir que los usuarios se autentiquen:</span><span class="sxs-lookup"><span data-stu-id="21c5d-163">Set the default authentication policy to require users to be authenticated:</span></span>

[!code-csharp[](secure-data/samples/final3/Startup.cs?name=snippet&highlight=15-99)] 

 <span data-ttu-id="21c5d-164">Puede rechazar la autenticación en el nivel de método de página de Razor, controlador o acción con el `[AllowAnonymous]` atributo.</span><span class="sxs-lookup"><span data-stu-id="21c5d-164">You can opt out of authentication at the Razor Page, controller, or action method level with the `[AllowAnonymous]` attribute.</span></span> <span data-ttu-id="21c5d-165">Configuración de la directiva de autenticación predeterminado para exigir que los usuarios se autentiquen protege recién agregada de las páginas de Razor y controladores.</span><span class="sxs-lookup"><span data-stu-id="21c5d-165">Setting the default authentication policy to require users to be authenticated protects newly added Razor Pages and controllers.</span></span> <span data-ttu-id="21c5d-166">Tener la autenticación requerida por el valor predeterminado es más segura que confiar en los nuevos controladores y las páginas de Razor debe incluir el `[Authorize]` atributo.</span><span class="sxs-lookup"><span data-stu-id="21c5d-166">Having authentication required by default is more secure than relying on new controllers and Razor Pages to include the `[Authorize]` attribute.</span></span>

<span data-ttu-id="21c5d-167">Agregue [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) a las páginas de índice y privacidad para que los usuarios anónimos puedan obtener información sobre el sitio antes de registrarse.</span><span class="sxs-lookup"><span data-stu-id="21c5d-167">Add [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) to the Index and Privacy pages so anonymous users can get information about the site before they register.</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Index.cshtml.cs?highlight=1,7)]

### <a name="configure-the-test-account"></a><span data-ttu-id="21c5d-168">Configurar la cuenta de prueba</span><span class="sxs-lookup"><span data-stu-id="21c5d-168">Configure the test account</span></span>

<span data-ttu-id="21c5d-169">La `SeedData` clase crea dos cuentas: administrador y administrador.</span><span class="sxs-lookup"><span data-stu-id="21c5d-169">The `SeedData` class creates two accounts: administrator and manager.</span></span> <span data-ttu-id="21c5d-170">Use la [herramienta Secret Manager](xref:security/app-secrets) para establecer una contraseña para estas cuentas.</span><span class="sxs-lookup"><span data-stu-id="21c5d-170">Use the [Secret Manager tool](xref:security/app-secrets) to set a password for these accounts.</span></span> <span data-ttu-id="21c5d-171">Establecer la contraseña del directorio de proyecto (el directorio que contiene *Program.cs*):</span><span class="sxs-lookup"><span data-stu-id="21c5d-171">Set the password from the project directory (the directory containing *Program.cs*):</span></span>

```dotnetcli
dotnet user-secrets set SeedUserPW <PW>
```

<span data-ttu-id="21c5d-172">Si no se especifica una contraseña segura, se produce una excepción cuando `SeedData.Initialize` se llama.</span><span class="sxs-lookup"><span data-stu-id="21c5d-172">If a strong password is not specified, an exception is thrown when `SeedData.Initialize` is called.</span></span>

<span data-ttu-id="21c5d-173">Actualización `Main` usar la contraseña de la prueba:</span><span class="sxs-lookup"><span data-stu-id="21c5d-173">Update `Main` to use the test password:</span></span>

[!code-csharp[](secure-data/samples/final3/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a><span data-ttu-id="21c5d-174">Crear las cuentas de prueba y actualizar los contactos</span><span class="sxs-lookup"><span data-stu-id="21c5d-174">Create the test accounts and update the contacts</span></span>

<span data-ttu-id="21c5d-175">Actualización de la `Initialize` método en el `SeedData` clase para crear las cuentas de prueba:</span><span class="sxs-lookup"><span data-stu-id="21c5d-175">Update the `Initialize` method in the `SeedData` class to create the test accounts:</span></span>

[!code-csharp[](secure-data/samples/final3/Data/SeedData.cs?name=snippet_Initialize)]

<span data-ttu-id="21c5d-176">Agregue el identificador de usuario administrador y `ContactStatus` a los contactos.</span><span class="sxs-lookup"><span data-stu-id="21c5d-176">Add the administrator user ID and `ContactStatus` to the contacts.</span></span> <span data-ttu-id="21c5d-177">Realice uno de los contactos "Enviado" y un "rechazada".</span><span class="sxs-lookup"><span data-stu-id="21c5d-177">Make one of the contacts "Submitted" and one "Rejected".</span></span> <span data-ttu-id="21c5d-178">Agregue el Id. de usuario y el estado a todos los contactos.</span><span class="sxs-lookup"><span data-stu-id="21c5d-178">Add the user ID and status to all the contacts.</span></span> <span data-ttu-id="21c5d-179">Póngase en contacto un solo con se muestra:</span><span class="sxs-lookup"><span data-stu-id="21c5d-179">Only one contact is shown:</span></span>

[!code-csharp[](secure-data/samples/final3/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a><span data-ttu-id="21c5d-180">Crear controladores de autorización de administrador, administrador y propietario</span><span class="sxs-lookup"><span data-stu-id="21c5d-180">Create owner, manager, and administrator authorization handlers</span></span>

<span data-ttu-id="21c5d-181">Crear un `ContactIsOwnerAuthorizationHandler` clase en el *autorización* carpeta.</span><span class="sxs-lookup"><span data-stu-id="21c5d-181">Create a `ContactIsOwnerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="21c5d-182">El `ContactIsOwnerAuthorizationHandler` comprueba que el usuario que actúan en un recurso posee el recurso.</span><span class="sxs-lookup"><span data-stu-id="21c5d-182">The `ContactIsOwnerAuthorizationHandler` verifies that the user acting on a resource owns the resource.</span></span>

[!code-csharp[](secure-data/samples/final3/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

<span data-ttu-id="21c5d-183">El `ContactIsOwnerAuthorizationHandler` llamadas [contexto. Se realizan correctamente](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) si el usuario autenticado actual es el propietario del contacto.</span><span class="sxs-lookup"><span data-stu-id="21c5d-183">The `ContactIsOwnerAuthorizationHandler` calls [context.Succeed](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) if the current authenticated user is the contact owner.</span></span> <span data-ttu-id="21c5d-184">Controladores de autorización con carácter general:</span><span class="sxs-lookup"><span data-stu-id="21c5d-184">Authorization handlers generally:</span></span>

* <span data-ttu-id="21c5d-185">Devolver `context.Succeed` cuando se cumplen los requisitos.</span><span class="sxs-lookup"><span data-stu-id="21c5d-185">Return `context.Succeed` when the requirements are met.</span></span>
* <span data-ttu-id="21c5d-186">Devolver `Task.CompletedTask` cuando no se cumplen los requisitos.</span><span class="sxs-lookup"><span data-stu-id="21c5d-186">Return `Task.CompletedTask` when requirements aren't met.</span></span> <span data-ttu-id="21c5d-187">`Task.CompletedTask`no es correcto o erróneo&mdash;permite que se ejecuten otros controladores de autorización.</span><span class="sxs-lookup"><span data-stu-id="21c5d-187">`Task.CompletedTask` is not success or failure&mdash;it allows other authorization handlers to run.</span></span>

<span data-ttu-id="21c5d-188">Si necesita explícitamente un error, devolver [contexto. Un error](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span><span class="sxs-lookup"><span data-stu-id="21c5d-188">If you need to explicitly fail, return [context.Fail](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span></span>

<span data-ttu-id="21c5d-189">La aplicación permite a los propietarios de contacto para editar, eliminar o crear sus propios datos.</span><span class="sxs-lookup"><span data-stu-id="21c5d-189">The app allows contact owners to edit/delete/create their own data.</span></span> <span data-ttu-id="21c5d-190">`ContactIsOwnerAuthorizationHandler` no es necesario comprobar la operación que se pasa en el parámetro de requisito.</span><span class="sxs-lookup"><span data-stu-id="21c5d-190">`ContactIsOwnerAuthorizationHandler` doesn't need to check the operation passed in the requirement parameter.</span></span>

### <a name="create-a-manager-authorization-handler"></a><span data-ttu-id="21c5d-191">Crear un controlador de autorización de administrador</span><span class="sxs-lookup"><span data-stu-id="21c5d-191">Create a manager authorization handler</span></span>

<span data-ttu-id="21c5d-192">Crear un `ContactManagerAuthorizationHandler` clase en el *autorización* carpeta.</span><span class="sxs-lookup"><span data-stu-id="21c5d-192">Create a `ContactManagerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="21c5d-193">El `ContactManagerAuthorizationHandler` comprueba que el usuario que actúan en el recurso es un administrador.</span><span class="sxs-lookup"><span data-stu-id="21c5d-193">The `ContactManagerAuthorizationHandler` verifies the user acting on the resource is a manager.</span></span> <span data-ttu-id="21c5d-194">Solo los administradores pueden aprobar o rechazar los cambios de contenido (nuevos o modificados).</span><span class="sxs-lookup"><span data-stu-id="21c5d-194">Only managers can approve or reject content changes (new or changed).</span></span>

[!code-csharp[](secure-data/samples/final3/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a><span data-ttu-id="21c5d-195">Cree un controlador de autorización de administrador</span><span class="sxs-lookup"><span data-stu-id="21c5d-195">Create an administrator authorization handler</span></span>

<span data-ttu-id="21c5d-196">Crear un `ContactAdministratorsAuthorizationHandler` clase en el *autorización* carpeta.</span><span class="sxs-lookup"><span data-stu-id="21c5d-196">Create a `ContactAdministratorsAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="21c5d-197">El `ContactAdministratorsAuthorizationHandler` comprueba que el usuario que actúan en el recurso es un administrador.</span><span class="sxs-lookup"><span data-stu-id="21c5d-197">The `ContactAdministratorsAuthorizationHandler` verifies the user acting on the resource is an administrator.</span></span> <span data-ttu-id="21c5d-198">Administrador puede realizar todas las operaciones.</span><span class="sxs-lookup"><span data-stu-id="21c5d-198">Administrator can do all operations.</span></span>

[!code-csharp[](secure-data/samples/final3/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a><span data-ttu-id="21c5d-199">Registrar los controladores de autorización</span><span class="sxs-lookup"><span data-stu-id="21c5d-199">Register the authorization handlers</span></span>

<span data-ttu-id="21c5d-200">Se deben registrar los servicios mediante Entity Framework Core para [inserción de dependencias](xref:fundamentals/dependency-injection) mediante [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span><span class="sxs-lookup"><span data-stu-id="21c5d-200">Services using Entity Framework Core must be registered for [dependency injection](xref:fundamentals/dependency-injection) using [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span></span> <span data-ttu-id="21c5d-201">El `ContactIsOwnerAuthorizationHandler` utiliza ASP.NET Core [identidad](xref:security/authentication/identity), que se basa en Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="21c5d-201">The `ContactIsOwnerAuthorizationHandler` uses ASP.NET Core [Identity](xref:security/authentication/identity), which is built on Entity Framework Core.</span></span> <span data-ttu-id="21c5d-202">Registre los controladores con la colección de servicios para que estén disponibles para el `ContactsController` a través de [inserción de dependencias](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="21c5d-202">Register the handlers with the service collection so they're available to the `ContactsController` through [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="21c5d-203">Agregue el código siguiente al final de `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="21c5d-203">Add the following code to the end of `ConfigureServices`:</span></span>

[!code-csharp[](secure-data/samples/final3/Startup.cs?name=snippet_defaultPolicy&highlight=23-99)]

<span data-ttu-id="21c5d-204">`ContactAdministratorsAuthorizationHandler` y `ContactManagerAuthorizationHandler` se agregan como singleton.</span><span class="sxs-lookup"><span data-stu-id="21c5d-204">`ContactAdministratorsAuthorizationHandler` and `ContactManagerAuthorizationHandler` are added as singletons.</span></span> <span data-ttu-id="21c5d-205">Lo singletons con estado porque no usan EF y toda la información necesaria se encuentra en la `Context` parámetro de la `HandleRequirementAsync` método.</span><span class="sxs-lookup"><span data-stu-id="21c5d-205">They're singletons because they don't use EF and all the information needed is in the `Context` parameter of the `HandleRequirementAsync` method.</span></span>

## <a name="support-authorization"></a><span data-ttu-id="21c5d-206">Admitir la autorización</span><span class="sxs-lookup"><span data-stu-id="21c5d-206">Support authorization</span></span>

<span data-ttu-id="21c5d-207">En esta sección, actualice las páginas de Razor y agregue una clase de los requisitos de operaciones.</span><span class="sxs-lookup"><span data-stu-id="21c5d-207">In this section, you update the Razor Pages and add an operations requirements class.</span></span>

### <a name="review-the-contact-operations-requirements-class"></a><span data-ttu-id="21c5d-208">Revisión de la clase de los requisitos de las operaciones de contacto</span><span class="sxs-lookup"><span data-stu-id="21c5d-208">Review the contact operations requirements class</span></span>

<span data-ttu-id="21c5d-209">Revise el `ContactOperations` clase.</span><span class="sxs-lookup"><span data-stu-id="21c5d-209">Review the `ContactOperations` class.</span></span> <span data-ttu-id="21c5d-210">Esta clase contiene los requisitos que admite la aplicación:</span><span class="sxs-lookup"><span data-stu-id="21c5d-210">This class contains the requirements the app supports:</span></span>

[!code-csharp[](secure-data/samples/final3/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-contacts-razor-pages"></a><span data-ttu-id="21c5d-211">Crear una clase base para las páginas de Razor de contactos</span><span class="sxs-lookup"><span data-stu-id="21c5d-211">Create a base class for the Contacts Razor Pages</span></span>

<span data-ttu-id="21c5d-212">Cree una clase base que contiene los servicios usados en los contactos de las páginas de Razor.</span><span class="sxs-lookup"><span data-stu-id="21c5d-212">Create a base class that contains the services used in the contacts Razor Pages.</span></span> <span data-ttu-id="21c5d-213">La clase base coloca el código de inicialización en una ubicación:</span><span class="sxs-lookup"><span data-stu-id="21c5d-213">The base class puts the initialization code in one location:</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/DI_BasePageModel.cs)]

<span data-ttu-id="21c5d-214">El código anterior:</span><span class="sxs-lookup"><span data-stu-id="21c5d-214">The preceding code:</span></span>

* <span data-ttu-id="21c5d-215">Agrega el `IAuthorizationService` service acceda a los controladores de autorización.</span><span class="sxs-lookup"><span data-stu-id="21c5d-215">Adds the `IAuthorizationService` service to access to the authorization handlers.</span></span>
* <span data-ttu-id="21c5d-216">Agrega la identidad `UserManager` service.</span><span class="sxs-lookup"><span data-stu-id="21c5d-216">Adds the Identity `UserManager` service.</span></span>
* <span data-ttu-id="21c5d-217">Agregue la `ApplicationDbContext`.</span><span class="sxs-lookup"><span data-stu-id="21c5d-217">Add the `ApplicationDbContext`.</span></span>

### <a name="update-the-createmodel"></a><span data-ttu-id="21c5d-218">Actualizar el CreateModel</span><span class="sxs-lookup"><span data-stu-id="21c5d-218">Update the CreateModel</span></span>

<span data-ttu-id="21c5d-219">Actualizar el constructor del modelo de página create para usar el `DI_BasePageModel` clase base:</span><span class="sxs-lookup"><span data-stu-id="21c5d-219">Update the create page model constructor to use the `DI_BasePageModel` base class:</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

<span data-ttu-id="21c5d-220">Actualización de la `CreateModel.OnPostAsync` método:</span><span class="sxs-lookup"><span data-stu-id="21c5d-220">Update the `CreateModel.OnPostAsync` method to:</span></span>

* <span data-ttu-id="21c5d-221">Agregue el identificador de usuario para el `Contact` modelo.</span><span class="sxs-lookup"><span data-stu-id="21c5d-221">Add the user ID to the `Contact` model.</span></span>
* <span data-ttu-id="21c5d-222">Llamar al controlador de autorización para comprobar que el usuario tiene permiso para crear contactos.</span><span class="sxs-lookup"><span data-stu-id="21c5d-222">Call the authorization handler to verify the user has permission to create contacts.</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a><span data-ttu-id="21c5d-223">Actualizar el IndexModel</span><span class="sxs-lookup"><span data-stu-id="21c5d-223">Update the IndexModel</span></span>

<span data-ttu-id="21c5d-224">Actualización de la `OnGetAsync` método por lo que solo contactos aprobados se muestran a los usuarios generales:</span><span class="sxs-lookup"><span data-stu-id="21c5d-224">Update the `OnGetAsync` method so only approved contacts are shown to general users:</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a><span data-ttu-id="21c5d-225">Actualizar el EditModel</span><span class="sxs-lookup"><span data-stu-id="21c5d-225">Update the EditModel</span></span>

<span data-ttu-id="21c5d-226">Agregar un controlador de autorización para comprobar que el usuario propietario del contacto.</span><span class="sxs-lookup"><span data-stu-id="21c5d-226">Add an authorization handler to verify the user owns the contact.</span></span> <span data-ttu-id="21c5d-227">Dado que se está validando la autorización de recursos, el `[Authorize]` atributo no es suficiente.</span><span class="sxs-lookup"><span data-stu-id="21c5d-227">Because resource authorization is being validated, the `[Authorize]` attribute is not enough.</span></span> <span data-ttu-id="21c5d-228">La aplicación no tiene acceso al recurso cuando se evalúan los atributos.</span><span class="sxs-lookup"><span data-stu-id="21c5d-228">The app doesn't have access to the resource when attributes are evaluated.</span></span> <span data-ttu-id="21c5d-229">Autorización basada en el recurso debe ser un imperativo.</span><span class="sxs-lookup"><span data-stu-id="21c5d-229">Resource-based authorization must be imperative.</span></span> <span data-ttu-id="21c5d-230">Las comprobaciones deben realizarse una vez que la aplicación tenga acceso al recurso, cargándolo en el modelo de página o cargándola en el controlador de sí mismo.</span><span class="sxs-lookup"><span data-stu-id="21c5d-230">Checks must be performed once the app has access to the resource, either by loading it in the page model or by loading it within the handler itself.</span></span> <span data-ttu-id="21c5d-231">Con frecuencia acceder al recurso pasando la clave de recurso.</span><span class="sxs-lookup"><span data-stu-id="21c5d-231">You frequently access the resource by passing in the resource key.</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a><span data-ttu-id="21c5d-232">Actualizar el DeleteModel</span><span class="sxs-lookup"><span data-stu-id="21c5d-232">Update the DeleteModel</span></span>

<span data-ttu-id="21c5d-233">Actualice el modelo de página de delete para usar el controlador de autorización para comprobar que el usuario tiene permiso delete en el contacto.</span><span class="sxs-lookup"><span data-stu-id="21c5d-233">Update the delete page model to use the authorization handler to verify the user has delete permission on the contact.</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a><span data-ttu-id="21c5d-234">Insertar el servicio de autorización en las vistas</span><span class="sxs-lookup"><span data-stu-id="21c5d-234">Inject the authorization service into the views</span></span>

<span data-ttu-id="21c5d-235">Actualmente, se muestra en la interfaz de usuario edita y elimina los vínculos para los contactos que no se puede modificar el usuario.</span><span class="sxs-lookup"><span data-stu-id="21c5d-235">Currently, the UI shows edit and delete links for contacts the user can't modify.</span></span>

<span data-ttu-id="21c5d-236">Inserte el servicio de autorización en el archivo *pages/_ViewImports. cshtml* para que esté disponible para todas las vistas:</span><span class="sxs-lookup"><span data-stu-id="21c5d-236">Inject the authorization service in the *Pages/_ViewImports.cshtml* file so it's available to all views:</span></span>

[!code-cshtml[](secure-data/samples/final3/Pages/_ViewImports.cshtml?highlight=6-99)]

<span data-ttu-id="21c5d-237">El marcado anterior agrega varios `using` instrucciones.</span><span class="sxs-lookup"><span data-stu-id="21c5d-237">The preceding markup adds several `using` statements.</span></span>

<span data-ttu-id="21c5d-238">Actualización de la **editar** y **eliminar** vincula en *Pages/Contacts/Index.cshtml* por lo que sólo se representen a los usuarios con los permisos adecuados:</span><span class="sxs-lookup"><span data-stu-id="21c5d-238">Update the **Edit** and **Delete** links in *Pages/Contacts/Index.cshtml* so they're only rendered for users with the appropriate permissions:</span></span>

[!code-cshtml[](secure-data/samples/final3/Pages/Contacts/Index.cshtml?highlight=34-36,62-999)]

> [!WARNING]
> <span data-ttu-id="21c5d-239">Ocultar los vínculos de los usuarios que no tienen permiso para cambiar los datos no proteger la aplicación.</span><span class="sxs-lookup"><span data-stu-id="21c5d-239">Hiding links from users that don't have permission to change data doesn't secure the app.</span></span> <span data-ttu-id="21c5d-240">Ocultar vínculos hace que la aplicación más fácil de usar mostrando vínculos solo es válido.</span><span class="sxs-lookup"><span data-stu-id="21c5d-240">Hiding links makes the app more user-friendly by displaying only valid links.</span></span> <span data-ttu-id="21c5d-241">Los usuarios pueden hack las direcciones URL generadas para invocar Editar y eliminar operaciones en los datos que no poseen.</span><span class="sxs-lookup"><span data-stu-id="21c5d-241">Users can hack the generated URLs to invoke edit and delete operations on data they don't own.</span></span> <span data-ttu-id="21c5d-242">La página de Razor o el controlador debe exigir comprobaciones de acceso para proteger los datos.</span><span class="sxs-lookup"><span data-stu-id="21c5d-242">The Razor Page or controller must enforce access checks to secure the data.</span></span>

### <a name="update-details"></a><span data-ttu-id="21c5d-243">Detalles de la actualización</span><span class="sxs-lookup"><span data-stu-id="21c5d-243">Update Details</span></span>

<span data-ttu-id="21c5d-244">Actualice la vista de detalles para que los administradores pueden aprobar o rechazar contactos:</span><span class="sxs-lookup"><span data-stu-id="21c5d-244">Update the details view so managers can approve or reject contacts:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml?name=snippet)]

<span data-ttu-id="21c5d-245">Actualice el modelo de página de detalles:</span><span class="sxs-lookup"><span data-stu-id="21c5d-245">Update the details page model:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="add-or-remove-a-user-to-a-role"></a><span data-ttu-id="21c5d-246">Agregar o quitar un usuario a un rol</span><span class="sxs-lookup"><span data-stu-id="21c5d-246">Add or remove a user to a role</span></span>

<span data-ttu-id="21c5d-247">Consulte [este problema](https://github.com/aspnet/AspNetCore.Docs/issues/8502) para obtener información sobre:</span><span class="sxs-lookup"><span data-stu-id="21c5d-247">See [this issue](https://github.com/aspnet/AspNetCore.Docs/issues/8502) for information on:</span></span>

* <span data-ttu-id="21c5d-248">Quita los privilegios de un usuario.</span><span class="sxs-lookup"><span data-stu-id="21c5d-248">Removing privileges from a user.</span></span> <span data-ttu-id="21c5d-249">Por ejemplo, silenciar a un usuario en una aplicación de chat.</span><span class="sxs-lookup"><span data-stu-id="21c5d-249">For example, muting a user in a chat app.</span></span>
* <span data-ttu-id="21c5d-250">Agregar privilegios a un usuario.</span><span class="sxs-lookup"><span data-stu-id="21c5d-250">Adding privileges to a user.</span></span>

## <a name="test-the-completed-app"></a><span data-ttu-id="21c5d-251">Probar la aplicación completada</span><span class="sxs-lookup"><span data-stu-id="21c5d-251">Test the completed app</span></span>

<span data-ttu-id="21c5d-252">Si ya no ha establecido una contraseña para cuentas de usuario inicializados, utilice el [herramienta Secret Manager](xref:security/app-secrets#secret-manager) para establecer una contraseña:</span><span class="sxs-lookup"><span data-stu-id="21c5d-252">If you haven't already set a password for seeded user accounts, use the [Secret Manager tool](xref:security/app-secrets#secret-manager) to set a password:</span></span>

* <span data-ttu-id="21c5d-253">Elija una contraseña segura: Use ocho o más caracteres y al menos un carácter, número y símbolo en mayúsculas.</span><span class="sxs-lookup"><span data-stu-id="21c5d-253">Choose a strong password: Use eight or more characters and at least one upper-case character, number, and symbol.</span></span> <span data-ttu-id="21c5d-254">Por ejemplo, `Passw0rd!` cumple los requisitos de contraseña segura.</span><span class="sxs-lookup"><span data-stu-id="21c5d-254">For example, `Passw0rd!` meets the strong password requirements.</span></span>
* <span data-ttu-id="21c5d-255">Ejecute el siguiente comando desde la carpeta del proyecto, donde `<PW>` es la contraseña:</span><span class="sxs-lookup"><span data-stu-id="21c5d-255">Execute the following command from the project's folder, where `<PW>` is the password:</span></span>

  ```dotnetcli
  dotnet user-secrets set SeedUserPW <PW>
  ```

<span data-ttu-id="21c5d-256">Si la aplicación tiene contactos:</span><span class="sxs-lookup"><span data-stu-id="21c5d-256">If the app has contacts:</span></span>

* <span data-ttu-id="21c5d-257">Eliminar todos los registros en el `Contact` tabla.</span><span class="sxs-lookup"><span data-stu-id="21c5d-257">Delete all of the records in the `Contact` table.</span></span>
* <span data-ttu-id="21c5d-258">Reinicie la aplicación para inicializar la base de datos.</span><span class="sxs-lookup"><span data-stu-id="21c5d-258">Restart the app to seed the database.</span></span>

<span data-ttu-id="21c5d-259">Es una manera fácil de probar la aplicación completa iniciar los tres distintos exploradores (o las sesiones de InPrivate o incognito).</span><span class="sxs-lookup"><span data-stu-id="21c5d-259">An easy way to test the completed app is to launch three different browsers (or incognito/InPrivate sessions).</span></span> <span data-ttu-id="21c5d-260">En un explorador, registre un nuevo usuario (por ejemplo, `test@example.com`).</span><span class="sxs-lookup"><span data-stu-id="21c5d-260">In one browser, register a new user (for example, `test@example.com`).</span></span> <span data-ttu-id="21c5d-261">Inicie sesión con un usuario diferente en cada explorador.</span><span class="sxs-lookup"><span data-stu-id="21c5d-261">Sign in to each browser with a different user.</span></span> <span data-ttu-id="21c5d-262">Compruebe las siguientes operaciones:</span><span class="sxs-lookup"><span data-stu-id="21c5d-262">Verify the following operations:</span></span>

* <span data-ttu-id="21c5d-263">Usuarios registrados pueden ver todos los datos de contacto aprobados.</span><span class="sxs-lookup"><span data-stu-id="21c5d-263">Registered users can view all of the approved contact data.</span></span>
* <span data-ttu-id="21c5d-264">Los usuarios registrados pueden editar o eliminar sus propios datos.</span><span class="sxs-lookup"><span data-stu-id="21c5d-264">Registered users can edit/delete their own data.</span></span>
* <span data-ttu-id="21c5d-265">Los administradores pueden aprobar o rechazar datos de contacto.</span><span class="sxs-lookup"><span data-stu-id="21c5d-265">Managers can approve/reject contact data.</span></span> <span data-ttu-id="21c5d-266">El `Details` ver muestra **aprobar** y **rechazar** botones.</span><span class="sxs-lookup"><span data-stu-id="21c5d-266">The `Details` view shows **Approve** and **Reject** buttons.</span></span>
* <span data-ttu-id="21c5d-267">Los administradores pueden aprobar o rechazar y editar o eliminar todos los datos.</span><span class="sxs-lookup"><span data-stu-id="21c5d-267">Administrators can approve/reject and edit/delete all data.</span></span>

| <span data-ttu-id="21c5d-268">Usuario</span><span class="sxs-lookup"><span data-stu-id="21c5d-268">User</span></span>                | <span data-ttu-id="21c5d-269">Propagadas por la aplicación</span><span class="sxs-lookup"><span data-stu-id="21c5d-269">Seeded by the app</span></span> | <span data-ttu-id="21c5d-270">Opciones</span><span class="sxs-lookup"><span data-stu-id="21c5d-270">Options</span></span>                                  |
| ------------------- | :---------------: | ---------------------------------------- |
| test@example.com    | <span data-ttu-id="21c5d-271">No</span><span class="sxs-lookup"><span data-stu-id="21c5d-271">No</span></span>                | <span data-ttu-id="21c5d-272">Editar o eliminar los datos propios.</span><span class="sxs-lookup"><span data-stu-id="21c5d-272">Edit/delete the own data.</span></span>                |
| manager@contoso.com | <span data-ttu-id="21c5d-273">Sí</span><span class="sxs-lookup"><span data-stu-id="21c5d-273">Yes</span></span>               | <span data-ttu-id="21c5d-274">Aprobar o rechazar y editar o eliminar los datos propios.</span><span class="sxs-lookup"><span data-stu-id="21c5d-274">Approve/reject and edit/delete own data.</span></span> |
| admin@contoso.com   | <span data-ttu-id="21c5d-275">Sí</span><span class="sxs-lookup"><span data-stu-id="21c5d-275">Yes</span></span>               | <span data-ttu-id="21c5d-276">Aprobar o rechazar y editar o eliminar todos los datos.</span><span class="sxs-lookup"><span data-stu-id="21c5d-276">Approve/reject and edit/delete all data.</span></span> |

<span data-ttu-id="21c5d-277">Crear un contacto en el explorador del administrador.</span><span class="sxs-lookup"><span data-stu-id="21c5d-277">Create a contact in the administrator's browser.</span></span> <span data-ttu-id="21c5d-278">Copie la dirección URL para su eliminación y editar en el contacto del administrador.</span><span class="sxs-lookup"><span data-stu-id="21c5d-278">Copy the URL for delete and edit from the administrator contact.</span></span> <span data-ttu-id="21c5d-279">Pegue estos vínculos en el explorador del usuario de prueba para comprobar que el usuario de prueba no puede realizar estas operaciones.</span><span class="sxs-lookup"><span data-stu-id="21c5d-279">Paste these links into the test user's browser to verify the test user can't perform these operations.</span></span>

## <a name="create-the-starter-app"></a><span data-ttu-id="21c5d-280">Crear la aplicación de inicio</span><span class="sxs-lookup"><span data-stu-id="21c5d-280">Create the starter app</span></span>

* <span data-ttu-id="21c5d-281">Cree una aplicación de páginas de Razor denominada "ContactManager"</span><span class="sxs-lookup"><span data-stu-id="21c5d-281">Create a Razor Pages app named "ContactManager"</span></span>
  * <span data-ttu-id="21c5d-282">Creación de la aplicación con **cuentas de usuario individuales**.</span><span class="sxs-lookup"><span data-stu-id="21c5d-282">Create the app with **Individual User Accounts**.</span></span>
  * <span data-ttu-id="21c5d-283">Asígnele el nombre "ContactManager" para el espacio de nombres coincida con el espacio de nombres utilizado en el ejemplo.</span><span class="sxs-lookup"><span data-stu-id="21c5d-283">Name it "ContactManager" so the namespace matches the namespace used in the sample.</span></span>
  * <span data-ttu-id="21c5d-284">`-uld` Especifica LocalDB en lugar de SQLite</span><span class="sxs-lookup"><span data-stu-id="21c5d-284">`-uld` specifies LocalDB instead of SQLite</span></span>

  ```dotnetcli
  dotnet new webapp -o ContactManager -au Individual -uld
  ```

* <span data-ttu-id="21c5d-285">Agregue *Models/contact. CS*:</span><span class="sxs-lookup"><span data-stu-id="21c5d-285">Add *Models/Contact.cs*:</span></span>

  [!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

* <span data-ttu-id="21c5d-286">Scaffold el `Contact` modelo.</span><span class="sxs-lookup"><span data-stu-id="21c5d-286">Scaffold the `Contact` model.</span></span>
* <span data-ttu-id="21c5d-287">Crear la migración inicial y actualizar la base de datos:</span><span class="sxs-lookup"><span data-stu-id="21c5d-287">Create initial migration and update the database:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet tool install -g dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
dotnet ef database drop -f
dotnet ef migrations add initial
dotnet ef database update
```

<span data-ttu-id="21c5d-288">Si experimenta un error con el `dotnet aspnet-codegenerator razorpage` comando, consulte [este problema de github](https://github.com/aspnet/Scaffolding/issues/984).</span><span class="sxs-lookup"><span data-stu-id="21c5d-288">If you experience a bug with the `dotnet aspnet-codegenerator razorpage` command, see [this GitHub issue](https://github.com/aspnet/Scaffolding/issues/984).</span></span>

* <span data-ttu-id="21c5d-289">Actualice el delimitador de **ContactManager** en el archivo *pages/Shared/_Layout. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="21c5d-289">Update the **ContactManager** anchor in the *Pages/Shared/_Layout.cshtml* file:</span></span>

 ```cshtml
<a class="navbar-brand" asp-area="" asp-page="/Contacts/Index">ContactManager</a>
  ```

* <span data-ttu-id="21c5d-290">Probar la aplicación mediante la creación, edición y eliminación de un contacto</span><span class="sxs-lookup"><span data-stu-id="21c5d-290">Test the app by creating, editing, and deleting a contact</span></span>

### <a name="seed-the-database"></a><span data-ttu-id="21c5d-291">Inicializar la base de datos</span><span class="sxs-lookup"><span data-stu-id="21c5d-291">Seed the database</span></span>

<span data-ttu-id="21c5d-292">Agregue la clase [SeedData](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter3/Data/SeedData.cs) a la carpeta de *datos* :</span><span class="sxs-lookup"><span data-stu-id="21c5d-292">Add the [SeedData](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter3/Data/SeedData.cs) class to the *Data* folder:</span></span>

[!code-csharp[](secure-data/samples/starter3/Data/SeedData.cs)]

<span data-ttu-id="21c5d-293">Llame a `SeedData.Initialize` desde `Main`:</span><span class="sxs-lookup"><span data-stu-id="21c5d-293">Call `SeedData.Initialize` from `Main`:</span></span>

[!code-csharp[](secure-data/samples/starter3/Program.cs)]

<span data-ttu-id="21c5d-294">Probar que la aplicación había propagado la base de datos.</span><span class="sxs-lookup"><span data-stu-id="21c5d-294">Test that the app seeded the database.</span></span> <span data-ttu-id="21c5d-295">Si no hay ninguna fila en la base de datos de contacto, no se ejecuta el método de inicialización.</span><span class="sxs-lookup"><span data-stu-id="21c5d-295">If there are any rows in the contact DB, the seed method doesn't run.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1 < aspnetcore-3.0"

<span data-ttu-id="21c5d-296">Este tutorial muestra cómo crear una aplicación web ASP.NET Core con los datos protegidos por autorización del usuario.</span><span class="sxs-lookup"><span data-stu-id="21c5d-296">This tutorial shows how to create an ASP.NET Core web app with user data protected by authorization.</span></span> <span data-ttu-id="21c5d-297">Muestra una lista de contactos que autentican los usuarios (registrados) ha creado.</span><span class="sxs-lookup"><span data-stu-id="21c5d-297">It displays a list of contacts that authenticated (registered) users have created.</span></span> <span data-ttu-id="21c5d-298">Hay tres grupos de seguridad:</span><span class="sxs-lookup"><span data-stu-id="21c5d-298">There are three security groups:</span></span>

* <span data-ttu-id="21c5d-299">**Usuarios registrados** puede ver todos los datos aprobados y pueden sus propios datos de editar o eliminar.</span><span class="sxs-lookup"><span data-stu-id="21c5d-299">**Registered users** can view all the approved data and can edit/delete their own data.</span></span>
* <span data-ttu-id="21c5d-300">**Los administradores de** puede aprobar o rechazar los datos de contacto.</span><span class="sxs-lookup"><span data-stu-id="21c5d-300">**Managers** can approve or reject contact data.</span></span> <span data-ttu-id="21c5d-301">Solo contactos aprobados son visibles para los usuarios.</span><span class="sxs-lookup"><span data-stu-id="21c5d-301">Only approved contacts are visible to users.</span></span>
* <span data-ttu-id="21c5d-302">**Los administradores** puede aprobar o rechazar y editar o eliminar todos los datos.</span><span class="sxs-lookup"><span data-stu-id="21c5d-302">**Administrators** can approve/reject and edit/delete any data.</span></span>

<span data-ttu-id="21c5d-303">En la siguiente imagen, el usuario Rick (`rick@example.com`) ha iniciado sesión.</span><span class="sxs-lookup"><span data-stu-id="21c5d-303">In the following image, user Rick (`rick@example.com`) is signed in.</span></span> <span data-ttu-id="21c5d-304">Rick solo puede ver los contactos aprobados y **editar**/**eliminar**/**crear nuevo** vínculos para sus contactos.</span><span class="sxs-lookup"><span data-stu-id="21c5d-304">Rick can only view approved contacts and **Edit**/**Delete**/**Create New** links for his contacts.</span></span> <span data-ttu-id="21c5d-305">El último registro, creado por Rick, muestra **editar** y **eliminar** vínculos.</span><span class="sxs-lookup"><span data-stu-id="21c5d-305">Only the last record, created by Rick, displays **Edit** and **Delete** links.</span></span> <span data-ttu-id="21c5d-306">Otros usuarios no verán el último registro hasta un administrador o un administrador cambia el estado a "Aprobado".</span><span class="sxs-lookup"><span data-stu-id="21c5d-306">Other users won't see the last record until a manager or administrator changes the status to "Approved".</span></span>

![Captura de pantalla con Rick ha iniciado sesión](secure-data/_static/rick.png)

<span data-ttu-id="21c5d-308">En la siguiente imagen, `manager@contoso.com` ha iniciado sesión y en el rol del administrador:</span><span class="sxs-lookup"><span data-stu-id="21c5d-308">In the following image, `manager@contoso.com` is signed in and in the manager's role:</span></span>

![Captura de pantalla mostrando manager@contoso.com iniciada](secure-data/_static/manager1.png)

<span data-ttu-id="21c5d-310">La siguiente imagen muestra a los administradores de la vista de detalles de un contacto:</span><span class="sxs-lookup"><span data-stu-id="21c5d-310">The following image shows the managers details view of a contact:</span></span>

![Vista del Administrador de un contacto](secure-data/_static/manager.png)

<span data-ttu-id="21c5d-312">El **aprobar** y **rechazar** solo se muestran los botones para administradores y administradores.</span><span class="sxs-lookup"><span data-stu-id="21c5d-312">The **Approve** and **Reject** buttons are only displayed for managers and administrators.</span></span>

<span data-ttu-id="21c5d-313">En la siguiente imagen, `admin@contoso.com` ha iniciado sesión y en el rol de administrador:</span><span class="sxs-lookup"><span data-stu-id="21c5d-313">In the following image, `admin@contoso.com` is signed in and in the administrator's role:</span></span>

![Captura de pantalla mostrando admin@contoso.com iniciada](secure-data/_static/admin.png)

<span data-ttu-id="21c5d-315">El administrador tiene todos los privilegios.</span><span class="sxs-lookup"><span data-stu-id="21c5d-315">The administrator has all privileges.</span></span> <span data-ttu-id="21c5d-316">Puede leer, editar o eliminar cualquier contacto y cambiar el estado de los contactos.</span><span class="sxs-lookup"><span data-stu-id="21c5d-316">She can read/edit/delete any contact and change the status of contacts.</span></span>

<span data-ttu-id="21c5d-317">Se ha creado la aplicación por [scaffolding](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model) siguiente `Contact` modelo:</span><span class="sxs-lookup"><span data-stu-id="21c5d-317">The app was created by [scaffolding](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model) the following `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

<span data-ttu-id="21c5d-318">El ejemplo contiene los siguientes controladores de autorización:</span><span class="sxs-lookup"><span data-stu-id="21c5d-318">The sample contains the following authorization handlers:</span></span>

* <span data-ttu-id="21c5d-319">`ContactIsOwnerAuthorizationHandler`: Garantiza que un usuario solo puede editar sus datos.</span><span class="sxs-lookup"><span data-stu-id="21c5d-319">`ContactIsOwnerAuthorizationHandler`: Ensures that a user can only edit their data.</span></span>
* <span data-ttu-id="21c5d-320">`ContactManagerAuthorizationHandler`: Permite a los administradores aprobar o rechazar contactos.</span><span class="sxs-lookup"><span data-stu-id="21c5d-320">`ContactManagerAuthorizationHandler`: Allows managers to approve or reject contacts.</span></span>
* <span data-ttu-id="21c5d-321">`ContactAdministratorsAuthorizationHandler`: Permite a los administradores aprobar o rechazar contactos y editar o eliminar contactos.</span><span class="sxs-lookup"><span data-stu-id="21c5d-321">`ContactAdministratorsAuthorizationHandler`: Allows administrators to approve or reject contacts and to edit/delete contacts.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="21c5d-322">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="21c5d-322">Prerequisites</span></span>

<span data-ttu-id="21c5d-323">En este tutorial se avanza.</span><span class="sxs-lookup"><span data-stu-id="21c5d-323">This tutorial is advanced.</span></span> <span data-ttu-id="21c5d-324">Debe estar familiarizado con:</span><span class="sxs-lookup"><span data-stu-id="21c5d-324">You should be familiar with:</span></span>

* [<span data-ttu-id="21c5d-325">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="21c5d-325">ASP.NET Core</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="21c5d-326">Autenticación</span><span class="sxs-lookup"><span data-stu-id="21c5d-326">Authentication</span></span>](xref:security/authentication/identity)
* [<span data-ttu-id="21c5d-327">Confirmación de cuentas y recuperación de contraseñas</span><span class="sxs-lookup"><span data-stu-id="21c5d-327">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="21c5d-328">Autorización</span><span class="sxs-lookup"><span data-stu-id="21c5d-328">Authorization</span></span>](xref:security/authorization/introduction)
* [<span data-ttu-id="21c5d-329">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="21c5d-329">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

## <a name="the-starter-and-completed-app"></a><span data-ttu-id="21c5d-330">El inicio y la aplicación completada</span><span class="sxs-lookup"><span data-stu-id="21c5d-330">The starter and completed app</span></span>

<span data-ttu-id="21c5d-331">[Descargar](xref:index#how-to-download-a-sample) el [completado](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples) app.</span><span class="sxs-lookup"><span data-stu-id="21c5d-331">[Download](xref:index#how-to-download-a-sample) the [completed](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples) app.</span></span> <span data-ttu-id="21c5d-332">[Prueba](#test-the-completed-app) la aplicación completa, por lo que se familiarice con sus características de seguridad.</span><span class="sxs-lookup"><span data-stu-id="21c5d-332">[Test](#test-the-completed-app) the completed app so you become familiar with its security features.</span></span>

### <a name="the-starter-app"></a><span data-ttu-id="21c5d-333">La aplicación de inicio</span><span class="sxs-lookup"><span data-stu-id="21c5d-333">The starter app</span></span>

<span data-ttu-id="21c5d-334">[Descargar](xref:index#how-to-download-a-sample) el [starter](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/) app.</span><span class="sxs-lookup"><span data-stu-id="21c5d-334">[Download](xref:index#how-to-download-a-sample) the [starter](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/) app.</span></span>

<span data-ttu-id="21c5d-335">Ejecute la aplicación, pulse el **ContactManager** vincular y compruebe que puede crear, editar y eliminar un contacto.</span><span class="sxs-lookup"><span data-stu-id="21c5d-335">Run the app, tap the **ContactManager** link, and verify you can create, edit, and delete a contact.</span></span>

## <a name="secure-user-data"></a><span data-ttu-id="21c5d-336">Proteger los datos de usuario</span><span class="sxs-lookup"><span data-stu-id="21c5d-336">Secure user data</span></span>

<span data-ttu-id="21c5d-337">Las siguientes secciones contienen todos los pasos principales para crear la aplicación de datos de usuario segura.</span><span class="sxs-lookup"><span data-stu-id="21c5d-337">The following sections have all the major steps to create the secure user data app.</span></span> <span data-ttu-id="21c5d-338">Resultarle útil consultar el proyecto completado.</span><span class="sxs-lookup"><span data-stu-id="21c5d-338">You may find it helpful to refer to the completed project.</span></span>

### <a name="tie-the-contact-data-to-the-user"></a><span data-ttu-id="21c5d-339">Vincular los datos de contacto para el usuario</span><span class="sxs-lookup"><span data-stu-id="21c5d-339">Tie the contact data to the user</span></span>

<span data-ttu-id="21c5d-340">Usar ASP.NET [identidad](xref:security/authentication/identity) Id. de usuario para garantizar que los usuarios puede editar sus datos, pero no otros datos de los usuarios.</span><span class="sxs-lookup"><span data-stu-id="21c5d-340">Use the ASP.NET [Identity](xref:security/authentication/identity) user ID to ensure users can edit their data, but not other users data.</span></span> <span data-ttu-id="21c5d-341">Agregar `OwnerID` y `ContactStatus` a la `Contact` modelo:</span><span class="sxs-lookup"><span data-stu-id="21c5d-341">Add `OwnerID` and `ContactStatus` to the `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Models/Contact.cs?name=snippet1&highlight=5-6,16-999)]

<span data-ttu-id="21c5d-342">`OwnerID` es el identificador del usuario desde el `AspNetUser` de tabla en la [identidad](xref:security/authentication/identity) base de datos.</span><span class="sxs-lookup"><span data-stu-id="21c5d-342">`OwnerID` is the user's ID from the `AspNetUser` table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="21c5d-343">El `Status` campo determina si un contacto es visible para los usuarios en general.</span><span class="sxs-lookup"><span data-stu-id="21c5d-343">The `Status` field determines if a contact is viewable by general users.</span></span>

<span data-ttu-id="21c5d-344">Crear una nueva migración y actualización de la base de datos:</span><span class="sxs-lookup"><span data-stu-id="21c5d-344">Create a new migration and update the database:</span></span>

```dotnetcli
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="add-role-services-to-identity"></a><span data-ttu-id="21c5d-345">Agregar servicios de función a la identidad</span><span class="sxs-lookup"><span data-stu-id="21c5d-345">Add Role services to Identity</span></span>

<span data-ttu-id="21c5d-346">Anexar [AddRoles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) para agregar servicios de rol:</span><span class="sxs-lookup"><span data-stu-id="21c5d-346">Append [AddRoles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) to add Role services:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet2&highlight=12)]

### <a name="require-authenticated-users"></a><span data-ttu-id="21c5d-347">Requerir que los usuarios autenticados</span><span class="sxs-lookup"><span data-stu-id="21c5d-347">Require authenticated users</span></span>

<span data-ttu-id="21c5d-348">Establezca la directiva de autenticación predeterminado para exigir que los usuarios se autentiquen:</span><span class="sxs-lookup"><span data-stu-id="21c5d-348">Set the default authentication policy to require users to be authenticated:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet&highlight=17-99)] 

 <span data-ttu-id="21c5d-349">Puede rechazar la autenticación en el nivel de método de página de Razor, controlador o acción con el `[AllowAnonymous]` atributo.</span><span class="sxs-lookup"><span data-stu-id="21c5d-349">You can opt out of authentication at the Razor Page, controller, or action method level with the `[AllowAnonymous]` attribute.</span></span> <span data-ttu-id="21c5d-350">Configuración de la directiva de autenticación predeterminado para exigir que los usuarios se autentiquen protege recién agregada de las páginas de Razor y controladores.</span><span class="sxs-lookup"><span data-stu-id="21c5d-350">Setting the default authentication policy to require users to be authenticated protects newly added Razor Pages and controllers.</span></span> <span data-ttu-id="21c5d-351">Tener la autenticación requerida por el valor predeterminado es más segura que confiar en los nuevos controladores y las páginas de Razor debe incluir el `[Authorize]` atributo.</span><span class="sxs-lookup"><span data-stu-id="21c5d-351">Having authentication required by default is more secure than relying on new controllers and Razor Pages to include the `[Authorize]` attribute.</span></span>

<span data-ttu-id="21c5d-352">Agregar [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) al índice, acerca de y póngase en contacto con páginas para los usuarios anónimos pueden obtener información sobre el sitio antes de que se registren.</span><span class="sxs-lookup"><span data-stu-id="21c5d-352">Add [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) to the Index, About, and Contact pages so anonymous users can get information about the site before they register.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Index.cshtml.cs?highlight=1,6)]

### <a name="configure-the-test-account"></a><span data-ttu-id="21c5d-353">Configurar la cuenta de prueba</span><span class="sxs-lookup"><span data-stu-id="21c5d-353">Configure the test account</span></span>

<span data-ttu-id="21c5d-354">La `SeedData` clase crea dos cuentas: administrador y administrador.</span><span class="sxs-lookup"><span data-stu-id="21c5d-354">The `SeedData` class creates two accounts: administrator and manager.</span></span> <span data-ttu-id="21c5d-355">Use la [herramienta Secret Manager](xref:security/app-secrets) para establecer una contraseña para estas cuentas.</span><span class="sxs-lookup"><span data-stu-id="21c5d-355">Use the [Secret Manager tool](xref:security/app-secrets) to set a password for these accounts.</span></span> <span data-ttu-id="21c5d-356">Establecer la contraseña del directorio de proyecto (el directorio que contiene *Program.cs*):</span><span class="sxs-lookup"><span data-stu-id="21c5d-356">Set the password from the project directory (the directory containing *Program.cs*):</span></span>

```dotnetcli
dotnet user-secrets set SeedUserPW <PW>
```

<span data-ttu-id="21c5d-357">Si no se especifica una contraseña segura, se produce una excepción cuando `SeedData.Initialize` se llama.</span><span class="sxs-lookup"><span data-stu-id="21c5d-357">If a strong password is not specified, an exception is thrown when `SeedData.Initialize` is called.</span></span>

<span data-ttu-id="21c5d-358">Actualización `Main` usar la contraseña de la prueba:</span><span class="sxs-lookup"><span data-stu-id="21c5d-358">Update `Main` to use the test password:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a><span data-ttu-id="21c5d-359">Crear las cuentas de prueba y actualizar los contactos</span><span class="sxs-lookup"><span data-stu-id="21c5d-359">Create the test accounts and update the contacts</span></span>

<span data-ttu-id="21c5d-360">Actualización de la `Initialize` método en el `SeedData` clase para crear las cuentas de prueba:</span><span class="sxs-lookup"><span data-stu-id="21c5d-360">Update the `Initialize` method in the `SeedData` class to create the test accounts:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Data/SeedData.cs?name=snippet_Initialize)]

<span data-ttu-id="21c5d-361">Agregue el identificador de usuario administrador y `ContactStatus` a los contactos.</span><span class="sxs-lookup"><span data-stu-id="21c5d-361">Add the administrator user ID and `ContactStatus` to the contacts.</span></span> <span data-ttu-id="21c5d-362">Realice uno de los contactos "Enviado" y un "rechazada".</span><span class="sxs-lookup"><span data-stu-id="21c5d-362">Make one of the contacts "Submitted" and one "Rejected".</span></span> <span data-ttu-id="21c5d-363">Agregue el Id. de usuario y el estado a todos los contactos.</span><span class="sxs-lookup"><span data-stu-id="21c5d-363">Add the user ID and status to all the contacts.</span></span> <span data-ttu-id="21c5d-364">Póngase en contacto un solo con se muestra:</span><span class="sxs-lookup"><span data-stu-id="21c5d-364">Only one contact is shown:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a><span data-ttu-id="21c5d-365">Crear controladores de autorización de administrador, administrador y propietario</span><span class="sxs-lookup"><span data-stu-id="21c5d-365">Create owner, manager, and administrator authorization handlers</span></span>

<span data-ttu-id="21c5d-366">Cree una carpeta de *autorización* y cree `ContactIsOwnerAuthorizationHandler` una clase en ella.</span><span class="sxs-lookup"><span data-stu-id="21c5d-366">Create an *Authorization* folder and create a `ContactIsOwnerAuthorizationHandler` class in it.</span></span> <span data-ttu-id="21c5d-367">El `ContactIsOwnerAuthorizationHandler` comprueba que el usuario que actúan en un recurso posee el recurso.</span><span class="sxs-lookup"><span data-stu-id="21c5d-367">The `ContactIsOwnerAuthorizationHandler` verifies that the user acting on a resource owns the resource.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

<span data-ttu-id="21c5d-368">El `ContactIsOwnerAuthorizationHandler` llamadas [contexto. Se realizan correctamente](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) si el usuario autenticado actual es el propietario del contacto.</span><span class="sxs-lookup"><span data-stu-id="21c5d-368">The `ContactIsOwnerAuthorizationHandler` calls [context.Succeed](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) if the current authenticated user is the contact owner.</span></span> <span data-ttu-id="21c5d-369">Controladores de autorización con carácter general:</span><span class="sxs-lookup"><span data-stu-id="21c5d-369">Authorization handlers generally:</span></span>

* <span data-ttu-id="21c5d-370">Devolver `context.Succeed` cuando se cumplen los requisitos.</span><span class="sxs-lookup"><span data-stu-id="21c5d-370">Return `context.Succeed` when the requirements are met.</span></span>
* <span data-ttu-id="21c5d-371">Devolver `Task.CompletedTask` cuando no se cumplen los requisitos.</span><span class="sxs-lookup"><span data-stu-id="21c5d-371">Return `Task.CompletedTask` when requirements aren't met.</span></span> <span data-ttu-id="21c5d-372">`Task.CompletedTask`no es correcto o erróneo&mdash;permite que se ejecuten otros controladores de autorización.</span><span class="sxs-lookup"><span data-stu-id="21c5d-372">`Task.CompletedTask` is not success or failure&mdash;it allows other authorization handlers to run.</span></span>

<span data-ttu-id="21c5d-373">Si necesita explícitamente un error, devolver [contexto. Un error](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span><span class="sxs-lookup"><span data-stu-id="21c5d-373">If you need to explicitly fail, return [context.Fail](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span></span>

<span data-ttu-id="21c5d-374">La aplicación permite a los propietarios de contacto para editar, eliminar o crear sus propios datos.</span><span class="sxs-lookup"><span data-stu-id="21c5d-374">The app allows contact owners to edit/delete/create their own data.</span></span> <span data-ttu-id="21c5d-375">`ContactIsOwnerAuthorizationHandler` no es necesario comprobar la operación que se pasa en el parámetro de requisito.</span><span class="sxs-lookup"><span data-stu-id="21c5d-375">`ContactIsOwnerAuthorizationHandler` doesn't need to check the operation passed in the requirement parameter.</span></span>

### <a name="create-a-manager-authorization-handler"></a><span data-ttu-id="21c5d-376">Crear un controlador de autorización de administrador</span><span class="sxs-lookup"><span data-stu-id="21c5d-376">Create a manager authorization handler</span></span>

<span data-ttu-id="21c5d-377">Crear un `ContactManagerAuthorizationHandler` clase en el *autorización* carpeta.</span><span class="sxs-lookup"><span data-stu-id="21c5d-377">Create a `ContactManagerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="21c5d-378">El `ContactManagerAuthorizationHandler` comprueba que el usuario que actúan en el recurso es un administrador.</span><span class="sxs-lookup"><span data-stu-id="21c5d-378">The `ContactManagerAuthorizationHandler` verifies the user acting on the resource is a manager.</span></span> <span data-ttu-id="21c5d-379">Solo los administradores pueden aprobar o rechazar los cambios de contenido (nuevos o modificados).</span><span class="sxs-lookup"><span data-stu-id="21c5d-379">Only managers can approve or reject content changes (new or changed).</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a><span data-ttu-id="21c5d-380">Cree un controlador de autorización de administrador</span><span class="sxs-lookup"><span data-stu-id="21c5d-380">Create an administrator authorization handler</span></span>

<span data-ttu-id="21c5d-381">Crear un `ContactAdministratorsAuthorizationHandler` clase en el *autorización* carpeta.</span><span class="sxs-lookup"><span data-stu-id="21c5d-381">Create a `ContactAdministratorsAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="21c5d-382">El `ContactAdministratorsAuthorizationHandler` comprueba que el usuario que actúan en el recurso es un administrador.</span><span class="sxs-lookup"><span data-stu-id="21c5d-382">The `ContactAdministratorsAuthorizationHandler` verifies the user acting on the resource is an administrator.</span></span> <span data-ttu-id="21c5d-383">Administrador puede realizar todas las operaciones.</span><span class="sxs-lookup"><span data-stu-id="21c5d-383">Administrator can do all operations.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a><span data-ttu-id="21c5d-384">Registrar los controladores de autorización</span><span class="sxs-lookup"><span data-stu-id="21c5d-384">Register the authorization handlers</span></span>

<span data-ttu-id="21c5d-385">Se deben registrar los servicios mediante Entity Framework Core para [inserción de dependencias](xref:fundamentals/dependency-injection) mediante [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span><span class="sxs-lookup"><span data-stu-id="21c5d-385">Services using Entity Framework Core must be registered for [dependency injection](xref:fundamentals/dependency-injection) using [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span></span> <span data-ttu-id="21c5d-386">El `ContactIsOwnerAuthorizationHandler` utiliza ASP.NET Core [identidad](xref:security/authentication/identity), que se basa en Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="21c5d-386">The `ContactIsOwnerAuthorizationHandler` uses ASP.NET Core [Identity](xref:security/authentication/identity), which is built on Entity Framework Core.</span></span> <span data-ttu-id="21c5d-387">Registre los controladores con la colección de servicios para que estén disponibles para el `ContactsController` a través de [inserción de dependencias](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="21c5d-387">Register the handlers with the service collection so they're available to the `ContactsController` through [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="21c5d-388">Agregue el código siguiente al final de `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="21c5d-388">Add the following code to the end of `ConfigureServices`:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet_defaultPolicy&highlight=27-99)]

<span data-ttu-id="21c5d-389">`ContactAdministratorsAuthorizationHandler` y `ContactManagerAuthorizationHandler` se agregan como singleton.</span><span class="sxs-lookup"><span data-stu-id="21c5d-389">`ContactAdministratorsAuthorizationHandler` and `ContactManagerAuthorizationHandler` are added as singletons.</span></span> <span data-ttu-id="21c5d-390">Lo singletons con estado porque no usan EF y toda la información necesaria se encuentra en la `Context` parámetro de la `HandleRequirementAsync` método.</span><span class="sxs-lookup"><span data-stu-id="21c5d-390">They're singletons because they don't use EF and all the information needed is in the `Context` parameter of the `HandleRequirementAsync` method.</span></span>

## <a name="support-authorization"></a><span data-ttu-id="21c5d-391">Admitir la autorización</span><span class="sxs-lookup"><span data-stu-id="21c5d-391">Support authorization</span></span>

<span data-ttu-id="21c5d-392">En esta sección, actualice las páginas de Razor y agregue una clase de los requisitos de operaciones.</span><span class="sxs-lookup"><span data-stu-id="21c5d-392">In this section, you update the Razor Pages and add an operations requirements class.</span></span>

### <a name="review-the-contact-operations-requirements-class"></a><span data-ttu-id="21c5d-393">Revisión de la clase de los requisitos de las operaciones de contacto</span><span class="sxs-lookup"><span data-stu-id="21c5d-393">Review the contact operations requirements class</span></span>

<span data-ttu-id="21c5d-394">Revise el `ContactOperations` clase.</span><span class="sxs-lookup"><span data-stu-id="21c5d-394">Review the `ContactOperations` class.</span></span> <span data-ttu-id="21c5d-395">Esta clase contiene los requisitos que admite la aplicación:</span><span class="sxs-lookup"><span data-stu-id="21c5d-395">This class contains the requirements the app supports:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-contacts-razor-pages"></a><span data-ttu-id="21c5d-396">Crear una clase base para las páginas de Razor de contactos</span><span class="sxs-lookup"><span data-stu-id="21c5d-396">Create a base class for the Contacts Razor Pages</span></span>

<span data-ttu-id="21c5d-397">Cree una clase base que contiene los servicios usados en los contactos de las páginas de Razor.</span><span class="sxs-lookup"><span data-stu-id="21c5d-397">Create a base class that contains the services used in the contacts Razor Pages.</span></span> <span data-ttu-id="21c5d-398">La clase base coloca el código de inicialización en una ubicación:</span><span class="sxs-lookup"><span data-stu-id="21c5d-398">The base class puts the initialization code in one location:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/DI_BasePageModel.cs)]

<span data-ttu-id="21c5d-399">El código anterior:</span><span class="sxs-lookup"><span data-stu-id="21c5d-399">The preceding code:</span></span>

* <span data-ttu-id="21c5d-400">Agrega el `IAuthorizationService` service acceda a los controladores de autorización.</span><span class="sxs-lookup"><span data-stu-id="21c5d-400">Adds the `IAuthorizationService` service to access to the authorization handlers.</span></span>
* <span data-ttu-id="21c5d-401">Agrega la identidad `UserManager` service.</span><span class="sxs-lookup"><span data-stu-id="21c5d-401">Adds the Identity `UserManager` service.</span></span>
* <span data-ttu-id="21c5d-402">Agregue la `ApplicationDbContext`.</span><span class="sxs-lookup"><span data-stu-id="21c5d-402">Add the `ApplicationDbContext`.</span></span>

### <a name="update-the-createmodel"></a><span data-ttu-id="21c5d-403">Actualizar el CreateModel</span><span class="sxs-lookup"><span data-stu-id="21c5d-403">Update the CreateModel</span></span>

<span data-ttu-id="21c5d-404">Actualizar el constructor del modelo de página create para usar el `DI_BasePageModel` clase base:</span><span class="sxs-lookup"><span data-stu-id="21c5d-404">Update the create page model constructor to use the `DI_BasePageModel` base class:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

<span data-ttu-id="21c5d-405">Actualización de la `CreateModel.OnPostAsync` método:</span><span class="sxs-lookup"><span data-stu-id="21c5d-405">Update the `CreateModel.OnPostAsync` method to:</span></span>

* <span data-ttu-id="21c5d-406">Agregue el identificador de usuario para el `Contact` modelo.</span><span class="sxs-lookup"><span data-stu-id="21c5d-406">Add the user ID to the `Contact` model.</span></span>
* <span data-ttu-id="21c5d-407">Llamar al controlador de autorización para comprobar que el usuario tiene permiso para crear contactos.</span><span class="sxs-lookup"><span data-stu-id="21c5d-407">Call the authorization handler to verify the user has permission to create contacts.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a><span data-ttu-id="21c5d-408">Actualizar el IndexModel</span><span class="sxs-lookup"><span data-stu-id="21c5d-408">Update the IndexModel</span></span>

<span data-ttu-id="21c5d-409">Actualización de la `OnGetAsync` método por lo que solo contactos aprobados se muestran a los usuarios generales:</span><span class="sxs-lookup"><span data-stu-id="21c5d-409">Update the `OnGetAsync` method so only approved contacts are shown to general users:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a><span data-ttu-id="21c5d-410">Actualizar el EditModel</span><span class="sxs-lookup"><span data-stu-id="21c5d-410">Update the EditModel</span></span>

<span data-ttu-id="21c5d-411">Agregar un controlador de autorización para comprobar que el usuario propietario del contacto.</span><span class="sxs-lookup"><span data-stu-id="21c5d-411">Add an authorization handler to verify the user owns the contact.</span></span> <span data-ttu-id="21c5d-412">Dado que se está validando la autorización de recursos, el `[Authorize]` atributo no es suficiente.</span><span class="sxs-lookup"><span data-stu-id="21c5d-412">Because resource authorization is being validated, the `[Authorize]` attribute is not enough.</span></span> <span data-ttu-id="21c5d-413">La aplicación no tiene acceso al recurso cuando se evalúan los atributos.</span><span class="sxs-lookup"><span data-stu-id="21c5d-413">The app doesn't have access to the resource when attributes are evaluated.</span></span> <span data-ttu-id="21c5d-414">Autorización basada en el recurso debe ser un imperativo.</span><span class="sxs-lookup"><span data-stu-id="21c5d-414">Resource-based authorization must be imperative.</span></span> <span data-ttu-id="21c5d-415">Las comprobaciones deben realizarse una vez que la aplicación tenga acceso al recurso, cargándolo en el modelo de página o cargándola en el controlador de sí mismo.</span><span class="sxs-lookup"><span data-stu-id="21c5d-415">Checks must be performed once the app has access to the resource, either by loading it in the page model or by loading it within the handler itself.</span></span> <span data-ttu-id="21c5d-416">Con frecuencia acceder al recurso pasando la clave de recurso.</span><span class="sxs-lookup"><span data-stu-id="21c5d-416">You frequently access the resource by passing in the resource key.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a><span data-ttu-id="21c5d-417">Actualizar el DeleteModel</span><span class="sxs-lookup"><span data-stu-id="21c5d-417">Update the DeleteModel</span></span>

<span data-ttu-id="21c5d-418">Actualice el modelo de página de delete para usar el controlador de autorización para comprobar que el usuario tiene permiso delete en el contacto.</span><span class="sxs-lookup"><span data-stu-id="21c5d-418">Update the delete page model to use the authorization handler to verify the user has delete permission on the contact.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a><span data-ttu-id="21c5d-419">Insertar el servicio de autorización en las vistas</span><span class="sxs-lookup"><span data-stu-id="21c5d-419">Inject the authorization service into the views</span></span>

<span data-ttu-id="21c5d-420">Actualmente, se muestra en la interfaz de usuario edita y elimina los vínculos para los contactos que no se puede modificar el usuario.</span><span class="sxs-lookup"><span data-stu-id="21c5d-420">Currently, the UI shows edit and delete links for contacts the user can't modify.</span></span>

<span data-ttu-id="21c5d-421">Insertar el servicio de autorización en el *Views/_viewimports.cshtml* para que esté disponible para todas las vistas de archivos:</span><span class="sxs-lookup"><span data-stu-id="21c5d-421">Inject the authorization service in the *Views/_ViewImports.cshtml* file so it's available to all views:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/_ViewImports.cshtml?highlight=6-99)]

<span data-ttu-id="21c5d-422">El marcado anterior agrega varios `using` instrucciones.</span><span class="sxs-lookup"><span data-stu-id="21c5d-422">The preceding markup adds several `using` statements.</span></span>

<span data-ttu-id="21c5d-423">Actualización de la **editar** y **eliminar** vincula en *Pages/Contacts/Index.cshtml* por lo que sólo se representen a los usuarios con los permisos adecuados:</span><span class="sxs-lookup"><span data-stu-id="21c5d-423">Update the **Edit** and **Delete** links in *Pages/Contacts/Index.cshtml* so they're only rendered for users with the appropriate permissions:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Index.cshtml?highlight=34-36,62-999)]

> [!WARNING]
> <span data-ttu-id="21c5d-424">Ocultar los vínculos de los usuarios que no tienen permiso para cambiar los datos no proteger la aplicación.</span><span class="sxs-lookup"><span data-stu-id="21c5d-424">Hiding links from users that don't have permission to change data doesn't secure the app.</span></span> <span data-ttu-id="21c5d-425">Ocultar vínculos hace que la aplicación más fácil de usar mostrando vínculos solo es válido.</span><span class="sxs-lookup"><span data-stu-id="21c5d-425">Hiding links makes the app more user-friendly by displaying only valid links.</span></span> <span data-ttu-id="21c5d-426">Los usuarios pueden hack las direcciones URL generadas para invocar Editar y eliminar operaciones en los datos que no poseen.</span><span class="sxs-lookup"><span data-stu-id="21c5d-426">Users can hack the generated URLs to invoke edit and delete operations on data they don't own.</span></span> <span data-ttu-id="21c5d-427">La página de Razor o el controlador debe exigir comprobaciones de acceso para proteger los datos.</span><span class="sxs-lookup"><span data-stu-id="21c5d-427">The Razor Page or controller must enforce access checks to secure the data.</span></span>

### <a name="update-details"></a><span data-ttu-id="21c5d-428">Detalles de la actualización</span><span class="sxs-lookup"><span data-stu-id="21c5d-428">Update Details</span></span>

<span data-ttu-id="21c5d-429">Actualice la vista de detalles para que los administradores pueden aprobar o rechazar contactos:</span><span class="sxs-lookup"><span data-stu-id="21c5d-429">Update the details view so managers can approve or reject contacts:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml?name=snippet)]

<span data-ttu-id="21c5d-430">Actualice el modelo de página de detalles:</span><span class="sxs-lookup"><span data-stu-id="21c5d-430">Update the details page model:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="add-or-remove-a-user-to-a-role"></a><span data-ttu-id="21c5d-431">Agregar o quitar un usuario a un rol</span><span class="sxs-lookup"><span data-stu-id="21c5d-431">Add or remove a user to a role</span></span>

<span data-ttu-id="21c5d-432">Consulte [este problema](https://github.com/aspnet/AspNetCore.Docs/issues/8502) para obtener información sobre:</span><span class="sxs-lookup"><span data-stu-id="21c5d-432">See [this issue](https://github.com/aspnet/AspNetCore.Docs/issues/8502) for information on:</span></span>

* <span data-ttu-id="21c5d-433">Quita los privilegios de un usuario.</span><span class="sxs-lookup"><span data-stu-id="21c5d-433">Removing privileges from a user.</span></span> <span data-ttu-id="21c5d-434">Por ejemplo, silenciar a un usuario en una aplicación de chat.</span><span class="sxs-lookup"><span data-stu-id="21c5d-434">For example, muting a user in a chat app.</span></span>
* <span data-ttu-id="21c5d-435">Agregar privilegios a un usuario.</span><span class="sxs-lookup"><span data-stu-id="21c5d-435">Adding privileges to a user.</span></span>

## <a name="test-the-completed-app"></a><span data-ttu-id="21c5d-436">Probar la aplicación completada</span><span class="sxs-lookup"><span data-stu-id="21c5d-436">Test the completed app</span></span>

<span data-ttu-id="21c5d-437">Si ya no ha establecido una contraseña para cuentas de usuario inicializados, utilice el [herramienta Secret Manager](xref:security/app-secrets#secret-manager) para establecer una contraseña:</span><span class="sxs-lookup"><span data-stu-id="21c5d-437">If you haven't already set a password for seeded user accounts, use the [Secret Manager tool](xref:security/app-secrets#secret-manager) to set a password:</span></span>

* <span data-ttu-id="21c5d-438">Elija una contraseña segura: Use ocho o más caracteres y al menos un carácter, número y símbolo en mayúsculas.</span><span class="sxs-lookup"><span data-stu-id="21c5d-438">Choose a strong password: Use eight or more characters and at least one upper-case character, number, and symbol.</span></span> <span data-ttu-id="21c5d-439">Por ejemplo, `Passw0rd!` cumple los requisitos de contraseña segura.</span><span class="sxs-lookup"><span data-stu-id="21c5d-439">For example, `Passw0rd!` meets the strong password requirements.</span></span>
* <span data-ttu-id="21c5d-440">Ejecute el siguiente comando desde la carpeta del proyecto, donde `<PW>` es la contraseña:</span><span class="sxs-lookup"><span data-stu-id="21c5d-440">Execute the following command from the project's folder, where `<PW>` is the password:</span></span>

  ```dotnetcli
  dotnet user-secrets set SeedUserPW <PW>
  ```

* <span data-ttu-id="21c5d-441">Quitar y actualizar la base de datos</span><span class="sxs-lookup"><span data-stu-id="21c5d-441">Drop and update the Database</span></span>

  ```dotnetcli
  dotnet ef database drop -f
  dotnet ef database update  
  ```

* <span data-ttu-id="21c5d-442">Reinicie la aplicación para inicializar la base de datos.</span><span class="sxs-lookup"><span data-stu-id="21c5d-442">Restart the app to seed the database.</span></span>

<span data-ttu-id="21c5d-443">Es una manera fácil de probar la aplicación completa iniciar los tres distintos exploradores (o las sesiones de InPrivate o incognito).</span><span class="sxs-lookup"><span data-stu-id="21c5d-443">An easy way to test the completed app is to launch three different browsers (or incognito/InPrivate sessions).</span></span> <span data-ttu-id="21c5d-444">En un explorador, registre un nuevo usuario (por ejemplo, `test@example.com`).</span><span class="sxs-lookup"><span data-stu-id="21c5d-444">In one browser, register a new user (for example, `test@example.com`).</span></span> <span data-ttu-id="21c5d-445">Inicie sesión con un usuario diferente en cada explorador.</span><span class="sxs-lookup"><span data-stu-id="21c5d-445">Sign in to each browser with a different user.</span></span> <span data-ttu-id="21c5d-446">Compruebe las siguientes operaciones:</span><span class="sxs-lookup"><span data-stu-id="21c5d-446">Verify the following operations:</span></span>

* <span data-ttu-id="21c5d-447">Usuarios registrados pueden ver todos los datos de contacto aprobados.</span><span class="sxs-lookup"><span data-stu-id="21c5d-447">Registered users can view all of the approved contact data.</span></span>
* <span data-ttu-id="21c5d-448">Los usuarios registrados pueden editar o eliminar sus propios datos.</span><span class="sxs-lookup"><span data-stu-id="21c5d-448">Registered users can edit/delete their own data.</span></span>
* <span data-ttu-id="21c5d-449">Los administradores pueden aprobar o rechazar datos de contacto.</span><span class="sxs-lookup"><span data-stu-id="21c5d-449">Managers can approve/reject contact data.</span></span> <span data-ttu-id="21c5d-450">El `Details` ver muestra **aprobar** y **rechazar** botones.</span><span class="sxs-lookup"><span data-stu-id="21c5d-450">The `Details` view shows **Approve** and **Reject** buttons.</span></span>
* <span data-ttu-id="21c5d-451">Los administradores pueden aprobar o rechazar y editar o eliminar todos los datos.</span><span class="sxs-lookup"><span data-stu-id="21c5d-451">Administrators can approve/reject and edit/delete all data.</span></span>

| <span data-ttu-id="21c5d-452">Usuario</span><span class="sxs-lookup"><span data-stu-id="21c5d-452">User</span></span>                | <span data-ttu-id="21c5d-453">Propagadas por la aplicación</span><span class="sxs-lookup"><span data-stu-id="21c5d-453">Seeded by the app</span></span> | <span data-ttu-id="21c5d-454">Opciones</span><span class="sxs-lookup"><span data-stu-id="21c5d-454">Options</span></span>                                  |
| ------------------- | :---------------: | ---------------------------------------- |
| test@example.com    | <span data-ttu-id="21c5d-455">No</span><span class="sxs-lookup"><span data-stu-id="21c5d-455">No</span></span>                | <span data-ttu-id="21c5d-456">Editar o eliminar los datos propios.</span><span class="sxs-lookup"><span data-stu-id="21c5d-456">Edit/delete the own data.</span></span>                |
| manager@contoso.com | <span data-ttu-id="21c5d-457">Sí</span><span class="sxs-lookup"><span data-stu-id="21c5d-457">Yes</span></span>               | <span data-ttu-id="21c5d-458">Aprobar o rechazar y editar o eliminar los datos propios.</span><span class="sxs-lookup"><span data-stu-id="21c5d-458">Approve/reject and edit/delete own data.</span></span> |
| admin@contoso.com   | <span data-ttu-id="21c5d-459">Sí</span><span class="sxs-lookup"><span data-stu-id="21c5d-459">Yes</span></span>               | <span data-ttu-id="21c5d-460">Aprobar o rechazar y editar o eliminar todos los datos.</span><span class="sxs-lookup"><span data-stu-id="21c5d-460">Approve/reject and edit/delete all data.</span></span> |

<span data-ttu-id="21c5d-461">Crear un contacto en el explorador del administrador.</span><span class="sxs-lookup"><span data-stu-id="21c5d-461">Create a contact in the administrator's browser.</span></span> <span data-ttu-id="21c5d-462">Copie la dirección URL para su eliminación y editar en el contacto del administrador.</span><span class="sxs-lookup"><span data-stu-id="21c5d-462">Copy the URL for delete and edit from the administrator contact.</span></span> <span data-ttu-id="21c5d-463">Pegue estos vínculos en el explorador del usuario de prueba para comprobar que el usuario de prueba no puede realizar estas operaciones.</span><span class="sxs-lookup"><span data-stu-id="21c5d-463">Paste these links into the test user's browser to verify the test user can't perform these operations.</span></span>

## <a name="create-the-starter-app"></a><span data-ttu-id="21c5d-464">Crear la aplicación de inicio</span><span class="sxs-lookup"><span data-stu-id="21c5d-464">Create the starter app</span></span>

* <span data-ttu-id="21c5d-465">Cree una aplicación de páginas de Razor denominada "ContactManager"</span><span class="sxs-lookup"><span data-stu-id="21c5d-465">Create a Razor Pages app named "ContactManager"</span></span>
  * <span data-ttu-id="21c5d-466">Creación de la aplicación con **cuentas de usuario individuales**.</span><span class="sxs-lookup"><span data-stu-id="21c5d-466">Create the app with **Individual User Accounts**.</span></span>
  * <span data-ttu-id="21c5d-467">Asígnele el nombre "ContactManager" para el espacio de nombres coincida con el espacio de nombres utilizado en el ejemplo.</span><span class="sxs-lookup"><span data-stu-id="21c5d-467">Name it "ContactManager" so the namespace matches the namespace used in the sample.</span></span>
  * <span data-ttu-id="21c5d-468">`-uld` Especifica LocalDB en lugar de SQLite</span><span class="sxs-lookup"><span data-stu-id="21c5d-468">`-uld` specifies LocalDB instead of SQLite</span></span>

  ```dotnetcli
  dotnet new webapp -o ContactManager -au Individual -uld
  ```

* <span data-ttu-id="21c5d-469">Agregue *Models/contact. CS*:</span><span class="sxs-lookup"><span data-stu-id="21c5d-469">Add *Models/Contact.cs*:</span></span>

  [!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

* <span data-ttu-id="21c5d-470">Scaffold el `Contact` modelo.</span><span class="sxs-lookup"><span data-stu-id="21c5d-470">Scaffold the `Contact` model.</span></span>
* <span data-ttu-id="21c5d-471">Crear la migración inicial y actualizar la base de datos:</span><span class="sxs-lookup"><span data-stu-id="21c5d-471">Create initial migration and update the database:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
  dotnet ef database drop -f
  dotnet ef migrations add initial
  dotnet ef database update
  ```

* <span data-ttu-id="21c5d-472">Actualización de la **ContactManager** anclar en el *Pages/_Layout.cshtml* archivo:</span><span class="sxs-lookup"><span data-stu-id="21c5d-472">Update the **ContactManager** anchor in the *Pages/_Layout.cshtml* file:</span></span>

  ```cshtml
  <a asp-page="/Contacts/Index" class="navbar-brand">ContactManager</a>
  ```

* <span data-ttu-id="21c5d-473">Probar la aplicación mediante la creación, edición y eliminación de un contacto</span><span class="sxs-lookup"><span data-stu-id="21c5d-473">Test the app by creating, editing, and deleting a contact</span></span>

### <a name="seed-the-database"></a><span data-ttu-id="21c5d-474">Inicializar la base de datos</span><span class="sxs-lookup"><span data-stu-id="21c5d-474">Seed the database</span></span>

<span data-ttu-id="21c5d-475">Agregar el [SeedData](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2.1/Data/SeedData.cs) clase a la *datos* carpeta.</span><span class="sxs-lookup"><span data-stu-id="21c5d-475">Add the [SeedData](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2.1/Data/SeedData.cs) class to the *Data* folder.</span></span>

<span data-ttu-id="21c5d-476">Llame a `SeedData.Initialize` desde `Main`:</span><span class="sxs-lookup"><span data-stu-id="21c5d-476">Call `SeedData.Initialize` from `Main`:</span></span>

[!code-csharp[](secure-data/samples/starter2.1/Program.cs?name=snippet)]

<span data-ttu-id="21c5d-477">Probar que la aplicación había propagado la base de datos.</span><span class="sxs-lookup"><span data-stu-id="21c5d-477">Test that the app seeded the database.</span></span> <span data-ttu-id="21c5d-478">Si no hay ninguna fila en la base de datos de contacto, no se ejecuta el método de inicialización.</span><span class="sxs-lookup"><span data-stu-id="21c5d-478">If there are any rows in the contact DB, the seed method doesn't run.</span></span>

::: moniker-end

<a name="secure-data-add-resources-label"></a>

### <a name="additional-resources"></a><span data-ttu-id="21c5d-479">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="21c5d-479">Additional resources</span></span>

* [<span data-ttu-id="21c5d-480">Compilar una aplicación web de .NET Core y SQL Database en Azure App Service</span><span class="sxs-lookup"><span data-stu-id="21c5d-480">Build a .NET Core and SQL Database web app in Azure App Service</span></span>](/azure/app-service/app-service-web-tutorial-dotnetcore-sqldb)
* <span data-ttu-id="21c5d-481">[Laboratorio de autorización de ASP.NET Core](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span><span class="sxs-lookup"><span data-stu-id="21c5d-481">[ASP.NET Core Authorization Lab](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span></span> <span data-ttu-id="21c5d-482">Este laboratorio explica con más detalle en las características de seguridad, presentadas en este tutorial.</span><span class="sxs-lookup"><span data-stu-id="21c5d-482">This lab goes into more detail on the security features introduced in this tutorial.</span></span>
* <xref:security/authorization/introduction>
* [<span data-ttu-id="21c5d-483">Autorización personalizada basada en directivas</span><span class="sxs-lookup"><span data-stu-id="21c5d-483">Custom policy-based authorization</span></span>](xref:security/authorization/policies)
