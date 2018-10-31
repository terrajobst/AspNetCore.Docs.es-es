---
title: Crear una aplicación ASP.NET Core con datos de usuario protegidos por autorización
author: rick-anderson
description: Obtenga información sobre cómo crear una aplicación de páginas de Razor con datos de usuario protegidos por autorización. Incluye HTTPS, autenticación, seguridad, ASP.NET Core Identity.
ms.author: riande
ms.date: 7/24/2018
uid: security/authorization/secure-data
ms.openlocfilehash: 185628d4e06c9b5ae7f2685c10ea9e46dd5abe92
ms.sourcegitcommit: 4a6bbe84db24c2f3dd2de065de418fde952c8d40
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2018
ms.locfileid: "50253226"
---
# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a><span data-ttu-id="9b10c-104">Crear una aplicación ASP.NET Core con datos de usuario protegidos por autorización</span><span class="sxs-lookup"><span data-stu-id="9b10c-104">Create an ASP.NET Core app with user data protected by authorization</span></span>

<span data-ttu-id="9b10c-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT) y [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="9b10c-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="9b10c-106">Consulte [este PDF](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) para la versión de ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="9b10c-106">See [this PDF](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core MVC version.</span></span> <span data-ttu-id="9b10c-107">La versión 1.1 de ASP.NET Core de este tutorial se encuentra en [esto](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data) carpeta.</span><span class="sxs-lookup"><span data-stu-id="9b10c-107">The ASP.NET Core 1.1 version of this tutorial is in [this](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data) folder.</span></span> <span data-ttu-id="9b10c-108">La 1.1 de ejemplo de ASP.NET Core se encuentra en la [ejemplos](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).</span><span class="sxs-lookup"><span data-stu-id="9b10c-108">The 1.1 ASP.NET Core sample is in the [samples](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="9b10c-109">Consulte [este pdf](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_July16_18.pdf)</span><span class="sxs-lookup"><span data-stu-id="9b10c-109">See [this pdf](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_July16_18.pdf)</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="9b10c-110">Este tutorial muestra cómo crear una aplicación web ASP.NET Core con los datos protegidos por autorización del usuario.</span><span class="sxs-lookup"><span data-stu-id="9b10c-110">This tutorial shows how to create an ASP.NET Core web app with user data protected by authorization.</span></span> <span data-ttu-id="9b10c-111">Muestra una lista de contactos que autentican los usuarios (registrados) ha creado.</span><span class="sxs-lookup"><span data-stu-id="9b10c-111">It displays a list of contacts that authenticated (registered) users have created.</span></span> <span data-ttu-id="9b10c-112">Hay tres grupos de seguridad:</span><span class="sxs-lookup"><span data-stu-id="9b10c-112">There are three security groups:</span></span>

* <span data-ttu-id="9b10c-113">**Usuarios registrados** puede ver todos los datos aprobados y pueden sus propios datos de editar o eliminar.</span><span class="sxs-lookup"><span data-stu-id="9b10c-113">**Registered users** can view all the approved data and can edit/delete their own data.</span></span>
* <span data-ttu-id="9b10c-114">**Los administradores de** puede aprobar o rechazar los datos de contacto.</span><span class="sxs-lookup"><span data-stu-id="9b10c-114">**Managers** can approve or reject contact data.</span></span> <span data-ttu-id="9b10c-115">Solo contactos aprobados son visibles para los usuarios.</span><span class="sxs-lookup"><span data-stu-id="9b10c-115">Only approved contacts are visible to users.</span></span>
* <span data-ttu-id="9b10c-116">**Los administradores** puede aprobar o rechazar y editar o eliminar todos los datos.</span><span class="sxs-lookup"><span data-stu-id="9b10c-116">**Administrators** can approve/reject and edit/delete any data.</span></span>

<span data-ttu-id="9b10c-117">En la siguiente imagen, el usuario Rick (`rick@example.com`) ha iniciado sesión.</span><span class="sxs-lookup"><span data-stu-id="9b10c-117">In the following image, user Rick (`rick@example.com`) is signed in.</span></span> <span data-ttu-id="9b10c-118">Rick solo puede ver los contactos aprobados y **editar**/**eliminar**/**crear nuevo** vínculos para sus contactos.</span><span class="sxs-lookup"><span data-stu-id="9b10c-118">Rick can only view approved contacts and **Edit**/**Delete**/**Create New** links for his contacts.</span></span> <span data-ttu-id="9b10c-119">El último registro, creado por Rick, muestra **editar** y **eliminar** vínculos.</span><span class="sxs-lookup"><span data-stu-id="9b10c-119">Only the last record, created by Rick, displays **Edit** and **Delete** links.</span></span> <span data-ttu-id="9b10c-120">Otros usuarios no verán el último registro hasta un administrador o un administrador cambia el estado a "Aprobado".</span><span class="sxs-lookup"><span data-stu-id="9b10c-120">Other users won't see the last record until a manager or administrator changes the status to "Approved".</span></span>

![imagen describe anterior](secure-data/_static/rick.png)

<span data-ttu-id="9b10c-122">En la siguiente imagen, `manager@contoso.com` se registre y del rol managers:</span><span class="sxs-lookup"><span data-stu-id="9b10c-122">In the following image, `manager@contoso.com` is signed in and in the managers role:</span></span>

![imagen describe anterior](secure-data/_static/manager1.png)

<span data-ttu-id="9b10c-124">La siguiente imagen muestra a los administradores de la vista de detalles de un contacto:</span><span class="sxs-lookup"><span data-stu-id="9b10c-124">The following image shows the managers details view of a contact:</span></span>

![imagen describe anterior](secure-data/_static/manager.png)

<span data-ttu-id="9b10c-126">El **aprobar** y **rechazar** solo se muestran los botones para administradores y administradores.</span><span class="sxs-lookup"><span data-stu-id="9b10c-126">The **Approve** and **Reject** buttons are only displayed for managers and administrators.</span></span>

<span data-ttu-id="9b10c-127">En la siguiente imagen, `admin@contoso.com` se registre y en la función Administradores:</span><span class="sxs-lookup"><span data-stu-id="9b10c-127">In the following image, `admin@contoso.com` is signed in and in the administrators role:</span></span>

![imagen describe anterior](secure-data/_static/admin.png)

<span data-ttu-id="9b10c-129">El administrador tiene todos los privilegios.</span><span class="sxs-lookup"><span data-stu-id="9b10c-129">The administrator has all privileges.</span></span> <span data-ttu-id="9b10c-130">Puede leer, editar o eliminar cualquier contacto y cambiar el estado de los contactos.</span><span class="sxs-lookup"><span data-stu-id="9b10c-130">She can read/edit/delete any contact and change the status of contacts.</span></span>

<span data-ttu-id="9b10c-131">Se ha creado la aplicación por [scaffolding](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller) siguiente `Contact` modelo:</span><span class="sxs-lookup"><span data-stu-id="9b10c-131">The app was created by [scaffolding](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller) the following `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet)]

<span data-ttu-id="9b10c-132">El ejemplo contiene los siguientes controladores de autorización:</span><span class="sxs-lookup"><span data-stu-id="9b10c-132">The sample contains the following authorization handlers:</span></span>

* <span data-ttu-id="9b10c-133">`ContactIsOwnerAuthorizationHandler`: Se garantiza que un usuario solo puede modificar sus datos.</span><span class="sxs-lookup"><span data-stu-id="9b10c-133">`ContactIsOwnerAuthorizationHandler`: Ensures that a user can only edit their data.</span></span>
* <span data-ttu-id="9b10c-134">`ContactManagerAuthorizationHandler`: Permite a los administradores aprobar o rechazar los contactos.</span><span class="sxs-lookup"><span data-stu-id="9b10c-134">`ContactManagerAuthorizationHandler`: Allows managers to approve or reject contacts.</span></span>
* <span data-ttu-id="9b10c-135">`ContactAdministratorsAuthorizationHandler`: Permite que los administradores pueden aprobar o rechazar los contactos y editar o eliminar contactos.</span><span class="sxs-lookup"><span data-stu-id="9b10c-135">`ContactAdministratorsAuthorizationHandler`: Allows administrators to approve or reject contacts and to edit/delete contacts.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9b10c-136">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="9b10c-136">Prerequisites</span></span>

<span data-ttu-id="9b10c-137">En este tutorial se avanza.</span><span class="sxs-lookup"><span data-stu-id="9b10c-137">This tutorial is advanced.</span></span> <span data-ttu-id="9b10c-138">Debe estar familiarizado con:</span><span class="sxs-lookup"><span data-stu-id="9b10c-138">You should be familiar with:</span></span>

* [<span data-ttu-id="9b10c-139">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9b10c-139">ASP.NET Core</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="9b10c-140">Autenticación</span><span class="sxs-lookup"><span data-stu-id="9b10c-140">Authentication</span></span>](xref:security/authentication/identity)
* [<span data-ttu-id="9b10c-141">Confirmación de cuentas y recuperación de contraseñas</span><span class="sxs-lookup"><span data-stu-id="9b10c-141">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="9b10c-142">Autorización</span><span class="sxs-lookup"><span data-stu-id="9b10c-142">Authorization</span></span>](xref:security/authorization/introduction)
* [<span data-ttu-id="9b10c-143">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="9b10c-143">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

::: moniker-end

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="9b10c-144">En ASP.NET Core 2.1, `User.IsInRole` se produce un error cuando se usa `AddDefaultIdentity`.</span><span class="sxs-lookup"><span data-stu-id="9b10c-144">In ASP.NET Core 2.1, `User.IsInRole` fails when using `AddDefaultIdentity`.</span></span> <span data-ttu-id="9b10c-145">Este tutorial se usa `AddDefaultIdentity` y, por tanto, requiere ASP.NET Core 2.2 o posterior.</span><span class="sxs-lookup"><span data-stu-id="9b10c-145">This tutorial uses `AddDefaultIdentity` and therefore requires ASP.NET Core 2.2 or later.</span></span> <span data-ttu-id="9b10c-146">Consulte [este problema de GitHub](https://github.com/aspnet/Identity/issues/1813#issuecomment-394543909) para una solución alternativa.</span><span class="sxs-lookup"><span data-stu-id="9b10c-146">See [this GitHub issue](https://github.com/aspnet/Identity/issues/1813#issuecomment-394543909) for a work-around.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="the-starter-and-completed-app"></a><span data-ttu-id="9b10c-147">El inicio y la aplicación completada</span><span class="sxs-lookup"><span data-stu-id="9b10c-147">The starter and completed app</span></span>

<span data-ttu-id="9b10c-148">[Descargar](xref:index#how-to-download-a-sample) el [completado](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2) app.</span><span class="sxs-lookup"><span data-stu-id="9b10c-148">[Download](xref:index#how-to-download-a-sample) the [completed](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2) app.</span></span> <span data-ttu-id="9b10c-149">[Prueba](#test-the-completed-app) la aplicación completa, por lo que se familiarice con sus características de seguridad.</span><span class="sxs-lookup"><span data-stu-id="9b10c-149">[Test](#test-the-completed-app) the completed app so you become familiar with its security features.</span></span>

### <a name="the-starter-app"></a><span data-ttu-id="9b10c-150">La aplicación de inicio</span><span class="sxs-lookup"><span data-stu-id="9b10c-150">The starter app</span></span>

<span data-ttu-id="9b10c-151">[Descargar](xref:index#how-to-download-a-sample) el [starter](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2) app.</span><span class="sxs-lookup"><span data-stu-id="9b10c-151">[Download](xref:index#how-to-download-a-sample) the [starter](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2) app.</span></span>

<span data-ttu-id="9b10c-152">Ejecute la aplicación, pulse el **ContactManager** vincular y compruebe que puede crear, editar y eliminar un contacto.</span><span class="sxs-lookup"><span data-stu-id="9b10c-152">Run the app, tap the **ContactManager** link, and verify you can create, edit, and delete a contact.</span></span>

## <a name="secure-user-data"></a><span data-ttu-id="9b10c-153">Proteger los datos de usuario</span><span class="sxs-lookup"><span data-stu-id="9b10c-153">Secure user data</span></span>

<span data-ttu-id="9b10c-154">Las siguientes secciones contienen todos los pasos principales para crear la aplicación de datos de usuario segura.</span><span class="sxs-lookup"><span data-stu-id="9b10c-154">The following sections have all the major steps to create the secure user data app.</span></span> <span data-ttu-id="9b10c-155">Resultarle útil consultar el proyecto completado.</span><span class="sxs-lookup"><span data-stu-id="9b10c-155">You may find it helpful to refer to the completed project.</span></span>

### <a name="tie-the-contact-data-to-the-user"></a><span data-ttu-id="9b10c-156">Vincular los datos de contacto para el usuario</span><span class="sxs-lookup"><span data-stu-id="9b10c-156">Tie the contact data to the user</span></span>

<span data-ttu-id="9b10c-157">Usar ASP.NET [identidad](xref:security/authentication/identity) Id. de usuario para garantizar que los usuarios puede editar sus datos, pero no otros datos de los usuarios.</span><span class="sxs-lookup"><span data-stu-id="9b10c-157">Use the ASP.NET [Identity](xref:security/authentication/identity) user ID to ensure users can edit their data, but not other users data.</span></span> <span data-ttu-id="9b10c-158">Agregar `OwnerID` y `ContactStatus` a la `Contact` modelo:</span><span class="sxs-lookup"><span data-stu-id="9b10c-158">Add `OwnerID` and `ContactStatus` to the `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Models/Contact.cs?name=snippet1&highlight=5-6,16-999)]

<span data-ttu-id="9b10c-159">`OwnerID` es el identificador del usuario desde el `AspNetUser` de tabla en la [identidad](xref:security/authentication/identity) base de datos.</span><span class="sxs-lookup"><span data-stu-id="9b10c-159">`OwnerID` is the user's ID from the `AspNetUser` table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="9b10c-160">El `Status` campo determina si un contacto es visible para los usuarios en general.</span><span class="sxs-lookup"><span data-stu-id="9b10c-160">The `Status` field determines if a contact is viewable by general users.</span></span>

<span data-ttu-id="9b10c-161">Crear una nueva migración y actualización de la base de datos:</span><span class="sxs-lookup"><span data-stu-id="9b10c-161">Create a new migration and update the database:</span></span>

```console
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="add-role-services-to-identity"></a><span data-ttu-id="9b10c-162">Agregar servicios de función a la identidad</span><span class="sxs-lookup"><span data-stu-id="9b10c-162">Add Role services to Identity</span></span>

<span data-ttu-id="9b10c-163">Anexar [AddRoles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) para agregar servicios de rol:</span><span class="sxs-lookup"><span data-stu-id="9b10c-163">Append [AddRoles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) to add Role services:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet2&highlight=12)]

### <a name="require-authenticated-users"></a><span data-ttu-id="9b10c-164">Requerir que los usuarios autenticados</span><span class="sxs-lookup"><span data-stu-id="9b10c-164">Require authenticated users</span></span>

<span data-ttu-id="9b10c-165">Establezca la directiva de autenticación predeterminado para exigir que los usuarios se autentiquen:</span><span class="sxs-lookup"><span data-stu-id="9b10c-165">Set the default authentication policy to require users to be authenticated:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet&highlight=17-99)] 

 <span data-ttu-id="9b10c-166">Puede rechazar la autenticación en el nivel de método de página de Razor, controlador o acción con el `[AllowAnonymous]` atributo.</span><span class="sxs-lookup"><span data-stu-id="9b10c-166">You can opt out of authentication at the Razor Page, controller, or action method level with the `[AllowAnonymous]` attribute.</span></span> <span data-ttu-id="9b10c-167">Configuración de la directiva de autenticación predeterminado para exigir que los usuarios se autentiquen protege recién agregada de las páginas de Razor y controladores.</span><span class="sxs-lookup"><span data-stu-id="9b10c-167">Setting the default authentication policy to require users to be authenticated protects newly added Razor Pages and controllers.</span></span> <span data-ttu-id="9b10c-168">Tener la autenticación requerida por el valor predeterminado es más segura que confiar en los nuevos controladores y las páginas de Razor debe incluir el `[Authorize]` atributo.</span><span class="sxs-lookup"><span data-stu-id="9b10c-168">Having authentication required by default is more secure than relying on new controllers and Razor Pages to include the `[Authorize]` attribute.</span></span>

<span data-ttu-id="9b10c-169">Agregar [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) al índice, acerca de y póngase en contacto con páginas para los usuarios anónimos pueden obtener información sobre el sitio antes de que se registren.</span><span class="sxs-lookup"><span data-stu-id="9b10c-169">Add [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) to the Index, About, and Contact pages so anonymous users can get information about the site before they register.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Index.cshtml.cs?highlight=1,6)]

### <a name="configure-the-test-account"></a><span data-ttu-id="9b10c-170">Configurar la cuenta de prueba</span><span class="sxs-lookup"><span data-stu-id="9b10c-170">Configure the test account</span></span>

<span data-ttu-id="9b10c-171">La `SeedData` clase crea dos cuentas: administrador y administrador.</span><span class="sxs-lookup"><span data-stu-id="9b10c-171">The `SeedData` class creates two accounts: administrator and manager.</span></span> <span data-ttu-id="9b10c-172">Use la [herramienta Secret Manager](xref:security/app-secrets) para establecer una contraseña para estas cuentas.</span><span class="sxs-lookup"><span data-stu-id="9b10c-172">Use the [Secret Manager tool](xref:security/app-secrets) to set a password for these accounts.</span></span> <span data-ttu-id="9b10c-173">Establecer la contraseña del directorio de proyecto (el directorio que contiene *Program.cs*):</span><span class="sxs-lookup"><span data-stu-id="9b10c-173">Set the password from the project directory (the directory containing *Program.cs*):</span></span>

```console
dotnet user-secrets set SeedUserPW <PW>
```

<span data-ttu-id="9b10c-174">Si no se especifica una contraseña segura, se produce una excepción cuando `SeedData.Initialize` se llama.</span><span class="sxs-lookup"><span data-stu-id="9b10c-174">If a strong password is not specified, an exception is thrown when `SeedData.Initialize` is called.</span></span>

<span data-ttu-id="9b10c-175">Actualización `Main` usar la contraseña de la prueba:</span><span class="sxs-lookup"><span data-stu-id="9b10c-175">Update `Main` to use the test password:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a><span data-ttu-id="9b10c-176">Crear las cuentas de prueba y actualizar los contactos</span><span class="sxs-lookup"><span data-stu-id="9b10c-176">Create the test accounts and update the contacts</span></span>

<span data-ttu-id="9b10c-177">Actualización de la `Initialize` método en el `SeedData` clase para crear las cuentas de prueba:</span><span class="sxs-lookup"><span data-stu-id="9b10c-177">Update the `Initialize` method in the `SeedData` class to create the test accounts:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Data/SeedData.cs?name=snippet_Initialize)]

<span data-ttu-id="9b10c-178">Agregue el identificador de usuario administrador y `ContactStatus` a los contactos.</span><span class="sxs-lookup"><span data-stu-id="9b10c-178">Add the administrator user ID and `ContactStatus` to the contacts.</span></span> <span data-ttu-id="9b10c-179">Realice uno de los contactos "Enviado" y un "rechazada".</span><span class="sxs-lookup"><span data-stu-id="9b10c-179">Make one of the contacts "Submitted" and one "Rejected".</span></span> <span data-ttu-id="9b10c-180">Agregue el Id. de usuario y el estado a todos los contactos.</span><span class="sxs-lookup"><span data-stu-id="9b10c-180">Add the user ID and status to all the contacts.</span></span> <span data-ttu-id="9b10c-181">Póngase en contacto un solo con se muestra:</span><span class="sxs-lookup"><span data-stu-id="9b10c-181">Only one contact is shown:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a><span data-ttu-id="9b10c-182">Crear controladores de autorización de administrador, administrador y propietario</span><span class="sxs-lookup"><span data-stu-id="9b10c-182">Create owner, manager, and administrator authorization handlers</span></span>

<span data-ttu-id="9b10c-183">Crear un `ContactIsOwnerAuthorizationHandler` clase en el *autorización* carpeta.</span><span class="sxs-lookup"><span data-stu-id="9b10c-183">Create a `ContactIsOwnerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="9b10c-184">El `ContactIsOwnerAuthorizationHandler` comprueba que el usuario que actúan en un recurso posee el recurso.</span><span class="sxs-lookup"><span data-stu-id="9b10c-184">The `ContactIsOwnerAuthorizationHandler` verifies that the user acting on a resource owns the resource.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

<span data-ttu-id="9b10c-185">El `ContactIsOwnerAuthorizationHandler` llamadas [contexto. Se realizan correctamente](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) si el usuario autenticado actual es el propietario del contacto.</span><span class="sxs-lookup"><span data-stu-id="9b10c-185">The `ContactIsOwnerAuthorizationHandler` calls [context.Succeed](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) if the current authenticated user is the contact owner.</span></span> <span data-ttu-id="9b10c-186">Controladores de autorización con carácter general:</span><span class="sxs-lookup"><span data-stu-id="9b10c-186">Authorization handlers generally:</span></span>

* <span data-ttu-id="9b10c-187">Devolver `context.Succeed` cuando se cumplen los requisitos.</span><span class="sxs-lookup"><span data-stu-id="9b10c-187">Return `context.Succeed` when the requirements are met.</span></span>
* <span data-ttu-id="9b10c-188">Devolver `Task.CompletedTask` cuando no se cumplen los requisitos.</span><span class="sxs-lookup"><span data-stu-id="9b10c-188">Return `Task.CompletedTask` when requirements aren't met.</span></span> <span data-ttu-id="9b10c-189">`Task.CompletedTask` ni éxito o error&mdash;permite que otros controladores de autorización ejecutar.</span><span class="sxs-lookup"><span data-stu-id="9b10c-189">`Task.CompletedTask` is neither success or failure&mdash;it allows other authorization handlers to run.</span></span>

<span data-ttu-id="9b10c-190">Si necesita explícitamente un error, devolver [contexto. Un error](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span><span class="sxs-lookup"><span data-stu-id="9b10c-190">If you need to explicitly fail, return [context.Fail](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span></span>

<span data-ttu-id="9b10c-191">La aplicación permite a los propietarios de contacto para editar, eliminar o crear sus propios datos.</span><span class="sxs-lookup"><span data-stu-id="9b10c-191">The app allows contact owners to edit/delete/create their own data.</span></span> <span data-ttu-id="9b10c-192">`ContactIsOwnerAuthorizationHandler` no es necesario comprobar la operación que se pasa en el parámetro de requisito.</span><span class="sxs-lookup"><span data-stu-id="9b10c-192">`ContactIsOwnerAuthorizationHandler` doesn't need to check the operation passed in the requirement parameter.</span></span>

### <a name="create-a-manager-authorization-handler"></a><span data-ttu-id="9b10c-193">Crear un controlador de autorización de administrador</span><span class="sxs-lookup"><span data-stu-id="9b10c-193">Create a manager authorization handler</span></span>

<span data-ttu-id="9b10c-194">Crear un `ContactManagerAuthorizationHandler` clase en el *autorización* carpeta.</span><span class="sxs-lookup"><span data-stu-id="9b10c-194">Create a `ContactManagerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="9b10c-195">El `ContactManagerAuthorizationHandler` comprueba que el usuario que actúan en el recurso es un administrador.</span><span class="sxs-lookup"><span data-stu-id="9b10c-195">The `ContactManagerAuthorizationHandler` verifies the user acting on the resource is a manager.</span></span> <span data-ttu-id="9b10c-196">Solo los administradores pueden aprobar o rechazar los cambios de contenido (nuevos o modificados).</span><span class="sxs-lookup"><span data-stu-id="9b10c-196">Only managers can approve or reject content changes (new or changed).</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a><span data-ttu-id="9b10c-197">Cree un controlador de autorización de administrador</span><span class="sxs-lookup"><span data-stu-id="9b10c-197">Create an administrator authorization handler</span></span>

<span data-ttu-id="9b10c-198">Crear un `ContactAdministratorsAuthorizationHandler` clase en el *autorización* carpeta.</span><span class="sxs-lookup"><span data-stu-id="9b10c-198">Create a `ContactAdministratorsAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="9b10c-199">El `ContactAdministratorsAuthorizationHandler` comprueba que el usuario que actúan en el recurso es un administrador.</span><span class="sxs-lookup"><span data-stu-id="9b10c-199">The `ContactAdministratorsAuthorizationHandler` verifies the user acting on the resource is an administrator.</span></span> <span data-ttu-id="9b10c-200">Administrador puede realizar todas las operaciones.</span><span class="sxs-lookup"><span data-stu-id="9b10c-200">Administrator can do all operations.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a><span data-ttu-id="9b10c-201">Registrar los controladores de autorización</span><span class="sxs-lookup"><span data-stu-id="9b10c-201">Register the authorization handlers</span></span>

<span data-ttu-id="9b10c-202">Se deben registrar los servicios mediante Entity Framework Core para [inserción de dependencias](xref:fundamentals/dependency-injection) mediante [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span><span class="sxs-lookup"><span data-stu-id="9b10c-202">Services using Entity Framework Core must be registered for [dependency injection](xref:fundamentals/dependency-injection) using [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span></span> <span data-ttu-id="9b10c-203">El `ContactIsOwnerAuthorizationHandler` utiliza ASP.NET Core [identidad](xref:security/authentication/identity), que se basa en Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="9b10c-203">The `ContactIsOwnerAuthorizationHandler` uses ASP.NET Core [Identity](xref:security/authentication/identity), which is built on Entity Framework Core.</span></span> <span data-ttu-id="9b10c-204">Registre los controladores con la colección de servicios para que estén disponibles para el `ContactsController` a través de [inserción de dependencias](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="9b10c-204">Register the handlers with the service collection so they're available to the `ContactsController` through [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="9b10c-205">Agregue el código siguiente al final de `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="9b10c-205">Add the following code to the end of `ConfigureServices`:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet_defaultPolicy&highlight=27-99)]

<span data-ttu-id="9b10c-206">`ContactAdministratorsAuthorizationHandler` y `ContactManagerAuthorizationHandler` se agregan como singleton.</span><span class="sxs-lookup"><span data-stu-id="9b10c-206">`ContactAdministratorsAuthorizationHandler` and `ContactManagerAuthorizationHandler` are added as singletons.</span></span> <span data-ttu-id="9b10c-207">Lo singletons con estado porque no usan EF y toda la información necesaria se encuentra en la `Context` parámetro de la `HandleRequirementAsync` método.</span><span class="sxs-lookup"><span data-stu-id="9b10c-207">They're singletons because they don't use EF and all the information needed is in the `Context` parameter of the `HandleRequirementAsync` method.</span></span>

## <a name="support-authorization"></a><span data-ttu-id="9b10c-208">Admitir la autorización</span><span class="sxs-lookup"><span data-stu-id="9b10c-208">Support authorization</span></span>

<span data-ttu-id="9b10c-209">En esta sección, actualice las páginas de Razor y agregue una clase de los requisitos de operaciones.</span><span class="sxs-lookup"><span data-stu-id="9b10c-209">In this section, you update the Razor Pages and add an operations requirements class.</span></span>

### <a name="review-the-contact-operations-requirements-class"></a><span data-ttu-id="9b10c-210">Revisión de la clase de los requisitos de las operaciones de contacto</span><span class="sxs-lookup"><span data-stu-id="9b10c-210">Review the contact operations requirements class</span></span>

<span data-ttu-id="9b10c-211">Revise el `ContactOperations` clase.</span><span class="sxs-lookup"><span data-stu-id="9b10c-211">Review the `ContactOperations` class.</span></span> <span data-ttu-id="9b10c-212">Esta clase contiene los requisitos que admite la aplicación:</span><span class="sxs-lookup"><span data-stu-id="9b10c-212">This class contains the requirements the app supports:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-contacts-razor-pages"></a><span data-ttu-id="9b10c-213">Crear una clase base para las páginas de Razor de contactos</span><span class="sxs-lookup"><span data-stu-id="9b10c-213">Create a base class for the Contacts Razor Pages</span></span>

<span data-ttu-id="9b10c-214">Cree una clase base que contiene los servicios usados en los contactos de las páginas de Razor.</span><span class="sxs-lookup"><span data-stu-id="9b10c-214">Create a base class that contains the services used in the contacts Razor Pages.</span></span> <span data-ttu-id="9b10c-215">La clase base coloca el código de inicialización en una ubicación:</span><span class="sxs-lookup"><span data-stu-id="9b10c-215">The base class puts the initialization code in one location:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/DI_BasePageModel.cs)]

<span data-ttu-id="9b10c-216">El código anterior:</span><span class="sxs-lookup"><span data-stu-id="9b10c-216">The preceding code:</span></span>

* <span data-ttu-id="9b10c-217">Agrega el `IAuthorizationService` service acceda a los controladores de autorización.</span><span class="sxs-lookup"><span data-stu-id="9b10c-217">Adds the `IAuthorizationService` service to access to the authorization handlers.</span></span>
* <span data-ttu-id="9b10c-218">Agrega la identidad `UserManager` service.</span><span class="sxs-lookup"><span data-stu-id="9b10c-218">Adds the Identity `UserManager` service.</span></span>
* <span data-ttu-id="9b10c-219">Agregue la `ApplicationDbContext`.</span><span class="sxs-lookup"><span data-stu-id="9b10c-219">Add the `ApplicationDbContext`.</span></span>

### <a name="update-the-createmodel"></a><span data-ttu-id="9b10c-220">Actualizar el CreateModel</span><span class="sxs-lookup"><span data-stu-id="9b10c-220">Update the CreateModel</span></span>

<span data-ttu-id="9b10c-221">Actualizar el constructor del modelo de página create para usar el `DI_BasePageModel` clase base:</span><span class="sxs-lookup"><span data-stu-id="9b10c-221">Update the create page model constructor to use the `DI_BasePageModel` base class:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

<span data-ttu-id="9b10c-222">Actualización de la `CreateModel.OnPostAsync` método:</span><span class="sxs-lookup"><span data-stu-id="9b10c-222">Update the `CreateModel.OnPostAsync` method to:</span></span>

* <span data-ttu-id="9b10c-223">Agregue el identificador de usuario para el `Contact` modelo.</span><span class="sxs-lookup"><span data-stu-id="9b10c-223">Add the user ID to the `Contact` model.</span></span>
* <span data-ttu-id="9b10c-224">Llamar al controlador de autorización para comprobar que el usuario tiene permiso para crear contactos.</span><span class="sxs-lookup"><span data-stu-id="9b10c-224">Call the authorization handler to verify the user has permission to create contacts.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a><span data-ttu-id="9b10c-225">Actualizar el IndexModel</span><span class="sxs-lookup"><span data-stu-id="9b10c-225">Update the IndexModel</span></span>

<span data-ttu-id="9b10c-226">Actualización de la `OnGetAsync` método por lo que solo contactos aprobados se muestran a los usuarios generales:</span><span class="sxs-lookup"><span data-stu-id="9b10c-226">Update the `OnGetAsync` method so only approved contacts are shown to general users:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a><span data-ttu-id="9b10c-227">Actualizar el EditModel</span><span class="sxs-lookup"><span data-stu-id="9b10c-227">Update the EditModel</span></span>

<span data-ttu-id="9b10c-228">Agregar un controlador de autorización para comprobar que el usuario propietario del contacto.</span><span class="sxs-lookup"><span data-stu-id="9b10c-228">Add an authorization handler to verify the user owns the contact.</span></span> <span data-ttu-id="9b10c-229">Dado que se está validando la autorización de recursos, el `[Authorize]` atributo no es suficiente.</span><span class="sxs-lookup"><span data-stu-id="9b10c-229">Because resource authorization is being validated, the `[Authorize]` attribute is not enough.</span></span> <span data-ttu-id="9b10c-230">La aplicación no tiene acceso al recurso cuando se evalúan los atributos.</span><span class="sxs-lookup"><span data-stu-id="9b10c-230">The app doesn't have access to the resource when attributes are evaluated.</span></span> <span data-ttu-id="9b10c-231">Autorización basada en el recurso debe ser un imperativo.</span><span class="sxs-lookup"><span data-stu-id="9b10c-231">Resource-based authorization must be imperative.</span></span> <span data-ttu-id="9b10c-232">Las comprobaciones deben realizarse una vez que la aplicación tenga acceso al recurso, cargándolo en el modelo de página o cargándola en el controlador de sí mismo.</span><span class="sxs-lookup"><span data-stu-id="9b10c-232">Checks must be performed once the app has access to the resource, either by loading it in the page model or by loading it within the handler itself.</span></span> <span data-ttu-id="9b10c-233">Con frecuencia acceder al recurso pasando la clave de recurso.</span><span class="sxs-lookup"><span data-stu-id="9b10c-233">You frequently access the resource by passing in the resource key.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a><span data-ttu-id="9b10c-234">Actualizar el DeleteModel</span><span class="sxs-lookup"><span data-stu-id="9b10c-234">Update the DeleteModel</span></span>

<span data-ttu-id="9b10c-235">Actualice el modelo de página de delete para usar el controlador de autorización para comprobar que el usuario tiene permiso delete en el contacto.</span><span class="sxs-lookup"><span data-stu-id="9b10c-235">Update the delete page model to use the authorization handler to verify the user has delete permission on the contact.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a><span data-ttu-id="9b10c-236">Insertar el servicio de autorización en las vistas</span><span class="sxs-lookup"><span data-stu-id="9b10c-236">Inject the authorization service into the views</span></span>

<span data-ttu-id="9b10c-237">Actualmente, se muestra en la interfaz de usuario edita y elimina los vínculos para los contactos que no se puede modificar el usuario.</span><span class="sxs-lookup"><span data-stu-id="9b10c-237">Currently, the UI shows edit and delete links for contacts the user can't modify.</span></span>

<span data-ttu-id="9b10c-238">Insertar el servicio de autorización en el *Views/_viewimports.cshtml* para que esté disponible para todas las vistas de archivos:</span><span class="sxs-lookup"><span data-stu-id="9b10c-238">Inject the authorization service in the *Views/_ViewImports.cshtml* file so it's available to all views:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/_ViewImports.cshtml?highlight=6-99)]

<span data-ttu-id="9b10c-239">El marcado anterior agrega varios `using` instrucciones.</span><span class="sxs-lookup"><span data-stu-id="9b10c-239">The preceding markup adds several `using` statements.</span></span>

<span data-ttu-id="9b10c-240">Actualización de la **editar** y **eliminar** vincula en *Pages/Contacts/Index.cshtml* por lo que sólo se representen a los usuarios con los permisos adecuados:</span><span class="sxs-lookup"><span data-stu-id="9b10c-240">Update the **Edit** and **Delete** links in *Pages/Contacts/Index.cshtml* so they're only rendered for users with the appropriate permissions:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Index.cshtml?highlight=34-36,62-999)]

> [!WARNING]
> <span data-ttu-id="9b10c-241">Ocultar los vínculos de los usuarios que no tienen permiso para cambiar los datos no proteger la aplicación.</span><span class="sxs-lookup"><span data-stu-id="9b10c-241">Hiding links from users that don't have permission to change data doesn't secure the app.</span></span> <span data-ttu-id="9b10c-242">Ocultar vínculos hace que la aplicación más fácil de usar mostrando vínculos solo es válido.</span><span class="sxs-lookup"><span data-stu-id="9b10c-242">Hiding links makes the app more user-friendly by displaying only valid links.</span></span> <span data-ttu-id="9b10c-243">Los usuarios pueden hack las direcciones URL generadas para invocar Editar y eliminar operaciones en los datos que no poseen.</span><span class="sxs-lookup"><span data-stu-id="9b10c-243">Users can hack the generated URLs to invoke edit and delete operations on data they don't own.</span></span> <span data-ttu-id="9b10c-244">La página de Razor o el controlador debe exigir comprobaciones de acceso para proteger los datos.</span><span class="sxs-lookup"><span data-stu-id="9b10c-244">The Razor Page or controller must enforce access checks to secure the data.</span></span>

### <a name="update-details"></a><span data-ttu-id="9b10c-245">Detalles de la actualización</span><span class="sxs-lookup"><span data-stu-id="9b10c-245">Update Details</span></span>

<span data-ttu-id="9b10c-246">Actualice la vista de detalles para que los administradores pueden aprobar o rechazar contactos:</span><span class="sxs-lookup"><span data-stu-id="9b10c-246">Update the details view so managers can approve or reject contacts:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml?name=snippet)]

<span data-ttu-id="9b10c-247">Actualice el modelo de página de detalles:</span><span class="sxs-lookup"><span data-stu-id="9b10c-247">Update the details page model:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="add-or-remove-a-user-to-a-role"></a><span data-ttu-id="9b10c-248">Agregar o quitar un usuario a un rol</span><span class="sxs-lookup"><span data-stu-id="9b10c-248">Add or remove a user to a role</span></span>

<span data-ttu-id="9b10c-249">Consulte [este problema](https://github.com/aspnet/Docs/issues/8502) para obtener información sobre:</span><span class="sxs-lookup"><span data-stu-id="9b10c-249">See [this issue](https://github.com/aspnet/Docs/issues/8502) for information on:</span></span>

* <span data-ttu-id="9b10c-250">Quita los privilegios de un usuario.</span><span class="sxs-lookup"><span data-stu-id="9b10c-250">Removing privileges from a user.</span></span> <span data-ttu-id="9b10c-251">Por ejemplo un usuario en una aplicación de chat de silencio.</span><span class="sxs-lookup"><span data-stu-id="9b10c-251">For example muting a user in a chat app.</span></span>
* <span data-ttu-id="9b10c-252">Agregar privilegios a un usuario.</span><span class="sxs-lookup"><span data-stu-id="9b10c-252">Adding privileges to a user.</span></span>

## <a name="test-the-completed-app"></a><span data-ttu-id="9b10c-253">Probar la aplicación completada</span><span class="sxs-lookup"><span data-stu-id="9b10c-253">Test the completed app</span></span>

<span data-ttu-id="9b10c-254">Si la aplicación tiene contactos:</span><span class="sxs-lookup"><span data-stu-id="9b10c-254">If the app has contacts:</span></span>

* <span data-ttu-id="9b10c-255">Eliminar todos los registros de la `Contact` tabla.</span><span class="sxs-lookup"><span data-stu-id="9b10c-255">Delete all the records in the `Contact` table.</span></span>
* <span data-ttu-id="9b10c-256">Reinicie la aplicación para inicializar la base de datos.</span><span class="sxs-lookup"><span data-stu-id="9b10c-256">Restart the app to seed the database.</span></span>

<span data-ttu-id="9b10c-257">Registrar un usuario para la exploración de los contactos.</span><span class="sxs-lookup"><span data-stu-id="9b10c-257">Register a user for browsing the contacts.</span></span>

<span data-ttu-id="9b10c-258">Es una manera fácil de probar la aplicación completa iniciar las tres diferentes exploradores (o versiones de InPrivate o incognito).</span><span class="sxs-lookup"><span data-stu-id="9b10c-258">An easy way to test the completed app is to launch three different browsers (or incognito/InPrivate versions).</span></span> <span data-ttu-id="9b10c-259">En un explorador, registre un nuevo usuario (por ejemplo, `test@example.com`).</span><span class="sxs-lookup"><span data-stu-id="9b10c-259">In one browser, register a new user (for example, `test@example.com`).</span></span> <span data-ttu-id="9b10c-260">Inicie sesión con un usuario diferente en cada explorador.</span><span class="sxs-lookup"><span data-stu-id="9b10c-260">Sign in to each browser with a different user.</span></span> <span data-ttu-id="9b10c-261">Compruebe las siguientes operaciones:</span><span class="sxs-lookup"><span data-stu-id="9b10c-261">Verify the following operations:</span></span>

* <span data-ttu-id="9b10c-262">Los usuarios registrados pueden ver todos los datos de contacto aprobados.</span><span class="sxs-lookup"><span data-stu-id="9b10c-262">Registered users can view all the approved contact data.</span></span>
* <span data-ttu-id="9b10c-263">Los usuarios registrados pueden editar o eliminar sus propios datos.</span><span class="sxs-lookup"><span data-stu-id="9b10c-263">Registered users can edit/delete their own data.</span></span>
* <span data-ttu-id="9b10c-264">Los administradores pueden aprobar o rechazar datos de contacto.</span><span class="sxs-lookup"><span data-stu-id="9b10c-264">Managers can approve or reject contact data.</span></span> <span data-ttu-id="9b10c-265">El `Details` ver muestra **aprobar** y **rechazar** botones.</span><span class="sxs-lookup"><span data-stu-id="9b10c-265">The `Details` view shows **Approve** and **Reject** buttons.</span></span>
* <span data-ttu-id="9b10c-266">Los administradores pueden aprobar o rechazar y editar o eliminar todos los datos.</span><span class="sxs-lookup"><span data-stu-id="9b10c-266">Administrators can approve/reject and edit/delete any data.</span></span>

| <span data-ttu-id="9b10c-267">Usuario</span><span class="sxs-lookup"><span data-stu-id="9b10c-267">User</span></span>| <span data-ttu-id="9b10c-268">Opciones</span><span class="sxs-lookup"><span data-stu-id="9b10c-268">Options</span></span> |
| ------------ | ---------|
| test@example.com | <span data-ttu-id="9b10c-269">Puede poseen los datos de editar o eliminar</span><span class="sxs-lookup"><span data-stu-id="9b10c-269">Can edit/delete own data</span></span> |
| manager@contoso.com | <span data-ttu-id="9b10c-270">Pueden aprobar o rechazar y editar o eliminar datos de poseer</span><span class="sxs-lookup"><span data-stu-id="9b10c-270">Can approve/reject and edit/delete own data</span></span> |
| admin@contoso.com | <span data-ttu-id="9b10c-271">Puede editar o eliminar y aprobar o rechazar todos los datos</span><span class="sxs-lookup"><span data-stu-id="9b10c-271">Can edit/delete and approve/reject all data</span></span>|

<span data-ttu-id="9b10c-272">Crear un contacto en el explorador del administrador.</span><span class="sxs-lookup"><span data-stu-id="9b10c-272">Create a contact in the administrator's browser.</span></span> <span data-ttu-id="9b10c-273">Copie la dirección URL para su eliminación y editar en el contacto del administrador.</span><span class="sxs-lookup"><span data-stu-id="9b10c-273">Copy the URL for delete and edit from the administrator contact.</span></span> <span data-ttu-id="9b10c-274">Pegue estos vínculos en el explorador del usuario de prueba para comprobar que el usuario de prueba no puede realizar estas operaciones.</span><span class="sxs-lookup"><span data-stu-id="9b10c-274">Paste these links into the test user's browser to verify the test user can't perform these operations.</span></span>

## <a name="create-the-starter-app"></a><span data-ttu-id="9b10c-275">Crear la aplicación de inicio</span><span class="sxs-lookup"><span data-stu-id="9b10c-275">Create the starter app</span></span>

* <span data-ttu-id="9b10c-276">Cree una aplicación de páginas de Razor denominada "ContactManager"</span><span class="sxs-lookup"><span data-stu-id="9b10c-276">Create a Razor Pages app named "ContactManager"</span></span>
   * <span data-ttu-id="9b10c-277">Creación de la aplicación con **cuentas de usuario individuales**.</span><span class="sxs-lookup"><span data-stu-id="9b10c-277">Create the app with **Individual User Accounts**.</span></span>
   * <span data-ttu-id="9b10c-278">Asígnele el nombre "ContactManager" para el espacio de nombres coincida con el espacio de nombres utilizado en el ejemplo.</span><span class="sxs-lookup"><span data-stu-id="9b10c-278">Name it "ContactManager" so the namespace matches the namespace used in the sample.</span></span>
   * <span data-ttu-id="9b10c-279">`-uld` Especifica LocalDB en lugar de SQLite</span><span class="sxs-lookup"><span data-stu-id="9b10c-279">`-uld` specifies LocalDB instead of SQLite</span></span>

  ```console
  dotnet new webapp -o ContactManager -au Individual -uld
  ```

* <span data-ttu-id="9b10c-280">Agregar *Models\Contact.cs*:</span><span class="sxs-lookup"><span data-stu-id="9b10c-280">Add *Models\Contact.cs*:</span></span>

  [!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

* <span data-ttu-id="9b10c-281">Scaffold el `Contact` modelo.</span><span class="sxs-lookup"><span data-stu-id="9b10c-281">Scaffold the `Contact` model.</span></span>
* <span data-ttu-id="9b10c-282">Crear la migración inicial y actualizar la base de datos:</span><span class="sxs-lookup"><span data-stu-id="9b10c-282">Create initial migration and update the database:</span></span>

```console
dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
dotnet ef database drop -f
dotnet ef migrations add initial
dotnet ef database update
```

* <span data-ttu-id="9b10c-283">Actualización de la **ContactManager** anclar en el *Pages/_Layout.cshtml* archivo:</span><span class="sxs-lookup"><span data-stu-id="9b10c-283">Update the **ContactManager** anchor in the *Pages/_Layout.cshtml* file:</span></span>

```cshtml
<a asp-page="/Contacts/Index" class="navbar-brand">ContactManager</a>
```

* <span data-ttu-id="9b10c-284">Probar la aplicación mediante la creación, edición y eliminación de un contacto</span><span class="sxs-lookup"><span data-stu-id="9b10c-284">Test the app by creating, editing, and deleting a contact</span></span>

### <a name="seed-the-database"></a><span data-ttu-id="9b10c-285">Inicializar la base de datos</span><span class="sxs-lookup"><span data-stu-id="9b10c-285">Seed the database</span></span>

<span data-ttu-id="9b10c-286">Agregar el [SeedData](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2.1/Data/SeedData.cs) clase a la *datos* carpeta.</span><span class="sxs-lookup"><span data-stu-id="9b10c-286">Add the [SeedData](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2.1/Data/SeedData.cs) class to the *Data* folder.</span></span>

<span data-ttu-id="9b10c-287">Llame a `SeedData.Initialize` desde `Main`:</span><span class="sxs-lookup"><span data-stu-id="9b10c-287">Call `SeedData.Initialize` from `Main`:</span></span>

[!code-csharp[](secure-data/samples/starter2.1/Program.cs?name=snippet)]

<span data-ttu-id="9b10c-288">Probar que la aplicación había propagado la base de datos.</span><span class="sxs-lookup"><span data-stu-id="9b10c-288">Test that the app seeded the database.</span></span> <span data-ttu-id="9b10c-289">Si no hay ninguna fila en la base de datos de contacto, no se ejecuta el método de inicialización.</span><span class="sxs-lookup"><span data-stu-id="9b10c-289">If there are any rows in the contact DB, the seed method doesn't run.</span></span>

<a name="secure-data-add-resources-label"></a>

### <a name="additional-resources"></a><span data-ttu-id="9b10c-290">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="9b10c-290">Additional resources</span></span>

* [<span data-ttu-id="9b10c-291">Compilar una aplicación web de .NET Core y SQL Database en Azure App Service</span><span class="sxs-lookup"><span data-stu-id="9b10c-291">Build a .NET Core and SQL Database web app in Azure App Service</span></span>](/azure/app-service/app-service-web-tutorial-dotnetcore-sqldb)
* <span data-ttu-id="9b10c-292">[Laboratorio de autorización de ASP.NET Core](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span><span class="sxs-lookup"><span data-stu-id="9b10c-292">[ASP.NET Core Authorization Lab](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span></span> <span data-ttu-id="9b10c-293">Este laboratorio explica con más detalle en las características de seguridad, presentadas en este tutorial.</span><span class="sxs-lookup"><span data-stu-id="9b10c-293">This lab goes into more detail on the security features introduced in this tutorial.</span></span>
* <xref:security/authorization/introduction>
* [<span data-ttu-id="9b10c-294">Autorización personalizada basada en directivas</span><span class="sxs-lookup"><span data-stu-id="9b10c-294">Custom policy-based authorization</span></span>](xref:security/authorization/policies)

::: moniker-end
