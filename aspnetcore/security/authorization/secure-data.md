---
title: Crear una aplicación ASP.NET Core con datos de usuario protegidos por autorización
author: rick-anderson
description: Obtenga información sobre cómo crear una aplicación de páginas de Razor con datos de usuario protegidos por autorización. Incluye HTTPS, autenticación, seguridad, ASP.NET Core Identity.
ms.author: riande
ms.date: 12/18/2018
ms.custom: mvc, seodec18
uid: security/authorization/secure-data
ms.openlocfilehash: 65c72d4dd457f85451796c5713bedebafec7a7de
ms.sourcegitcommit: 8157e5a351f49aeef3769f7d38b787b4386aad5f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/20/2019
ms.locfileid: "74239832"
---
# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a><span data-ttu-id="d19a5-104">Crear una aplicación ASP.NET Core con datos de usuario protegidos por autorización</span><span class="sxs-lookup"><span data-stu-id="d19a5-104">Create an ASP.NET Core app with user data protected by authorization</span></span>

<span data-ttu-id="d19a5-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT) y [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="d19a5-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="d19a5-106">Vea [este PDF](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) para la versión de ASP.net Core MVC.</span><span class="sxs-lookup"><span data-stu-id="d19a5-106">See [this PDF](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core MVC version.</span></span> <span data-ttu-id="d19a5-107">La versión ASP.NET Core 1,1 de este tutorial se encuentra en [esta](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data) carpeta.</span><span class="sxs-lookup"><span data-stu-id="d19a5-107">The ASP.NET Core 1.1 version of this tutorial is in [this](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data) folder.</span></span> <span data-ttu-id="d19a5-108">El ejemplo 1,1 ASP.NET Core está en los [ejemplos](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).</span><span class="sxs-lookup"><span data-stu-id="d19a5-108">The 1.1 ASP.NET Core sample is in the [samples](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="d19a5-109">Vea [este PDF](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_July16_18.pdf)</span><span class="sxs-lookup"><span data-stu-id="d19a5-109">See [this pdf](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_July16_18.pdf)</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="d19a5-110">Este tutorial muestra cómo crear una aplicación web ASP.NET Core con los datos protegidos por autorización del usuario.</span><span class="sxs-lookup"><span data-stu-id="d19a5-110">This tutorial shows how to create an ASP.NET Core web app with user data protected by authorization.</span></span> <span data-ttu-id="d19a5-111">Muestra una lista de contactos que autentican los usuarios (registrados) ha creado.</span><span class="sxs-lookup"><span data-stu-id="d19a5-111">It displays a list of contacts that authenticated (registered) users have created.</span></span> <span data-ttu-id="d19a5-112">Hay tres grupos de seguridad:</span><span class="sxs-lookup"><span data-stu-id="d19a5-112">There are three security groups:</span></span>

* <span data-ttu-id="d19a5-113">**Los usuarios registrados** pueden ver todos los datos aprobados y pueden editar o eliminar sus propios datos.</span><span class="sxs-lookup"><span data-stu-id="d19a5-113">**Registered users** can view all the approved data and can edit/delete their own data.</span></span>
* <span data-ttu-id="d19a5-114">Los **administradores** pueden aprobar o rechazar datos de contacto.</span><span class="sxs-lookup"><span data-stu-id="d19a5-114">**Managers** can approve or reject contact data.</span></span> <span data-ttu-id="d19a5-115">Solo contactos aprobados son visibles para los usuarios.</span><span class="sxs-lookup"><span data-stu-id="d19a5-115">Only approved contacts are visible to users.</span></span>
* <span data-ttu-id="d19a5-116">**Los administradores** pueden aprobar o rechazar y editar o eliminar los datos.</span><span class="sxs-lookup"><span data-stu-id="d19a5-116">**Administrators** can approve/reject and edit/delete any data.</span></span>

<span data-ttu-id="d19a5-117">Las imágenes de este documento no coinciden exactamente con las plantillas más recientes.</span><span class="sxs-lookup"><span data-stu-id="d19a5-117">The images in this document don't exactly match the latest templates.</span></span>

<span data-ttu-id="d19a5-118">En la siguiente imagen, el usuario Rick (`rick@example.com`) ha iniciado sesión.</span><span class="sxs-lookup"><span data-stu-id="d19a5-118">In the following image, user Rick (`rick@example.com`) is signed in.</span></span> <span data-ttu-id="d19a5-119">Rick solo puede ver contactos aprobados y **editar**/**eliminar**/**crear nuevos** vínculos para sus contactos.</span><span class="sxs-lookup"><span data-stu-id="d19a5-119">Rick can only view approved contacts and **Edit**/**Delete**/**Create New** links for his contacts.</span></span> <span data-ttu-id="d19a5-120">Solo el último registro, creado por Rick, muestra los vínculos de **edición** y **eliminación** .</span><span class="sxs-lookup"><span data-stu-id="d19a5-120">Only the last record, created by Rick, displays **Edit** and **Delete** links.</span></span> <span data-ttu-id="d19a5-121">Otros usuarios no verán el último registro hasta un administrador o un administrador cambia el estado a "Aprobado".</span><span class="sxs-lookup"><span data-stu-id="d19a5-121">Other users won't see the last record until a manager or administrator changes the status to "Approved".</span></span>

![Captura de pantalla con Rick ha iniciado sesión](secure-data/_static/rick.png)

<span data-ttu-id="d19a5-123">En la imagen siguiente, `manager@contoso.com` está conectado y en el rol del administrador:</span><span class="sxs-lookup"><span data-stu-id="d19a5-123">In the following image, `manager@contoso.com` is signed in and in the manager's role:</span></span>

![Captura de pantalla que muestra manager@contoso.com sesión iniciada](secure-data/_static/manager1.png)

<span data-ttu-id="d19a5-125">La siguiente imagen muestra a los administradores de la vista de detalles de un contacto:</span><span class="sxs-lookup"><span data-stu-id="d19a5-125">The following image shows the managers details view of a contact:</span></span>

![Vista del Administrador de un contacto](secure-data/_static/manager.png)

<span data-ttu-id="d19a5-127">Los botones **aprobar** y **rechazar** solo se muestran para administradores y administradores.</span><span class="sxs-lookup"><span data-stu-id="d19a5-127">The **Approve** and **Reject** buttons are only displayed for managers and administrators.</span></span>

<span data-ttu-id="d19a5-128">En la siguiente imagen, `admin@contoso.com` está conectado y en el rol de administrador:</span><span class="sxs-lookup"><span data-stu-id="d19a5-128">In the following image, `admin@contoso.com` is signed in and in the administrator's role:</span></span>

![Captura de pantalla que muestra admin@contoso.com sesión iniciada](secure-data/_static/admin.png)

<span data-ttu-id="d19a5-130">El administrador tiene todos los privilegios.</span><span class="sxs-lookup"><span data-stu-id="d19a5-130">The administrator has all privileges.</span></span> <span data-ttu-id="d19a5-131">Puede leer, editar o eliminar cualquier contacto y cambiar el estado de los contactos.</span><span class="sxs-lookup"><span data-stu-id="d19a5-131">She can read/edit/delete any contact and change the status of contacts.</span></span>

<span data-ttu-id="d19a5-132">La aplicación se creó mediante [scaffolding](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model) en el siguiente modelo de `Contact`:</span><span class="sxs-lookup"><span data-stu-id="d19a5-132">The app was created by [scaffolding](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model) the following `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

<span data-ttu-id="d19a5-133">El ejemplo contiene los siguientes controladores de autorización:</span><span class="sxs-lookup"><span data-stu-id="d19a5-133">The sample contains the following authorization handlers:</span></span>

* <span data-ttu-id="d19a5-134">`ContactIsOwnerAuthorizationHandler`: garantiza que un usuario solo pueda editar sus datos.</span><span class="sxs-lookup"><span data-stu-id="d19a5-134">`ContactIsOwnerAuthorizationHandler`: Ensures that a user can only edit their data.</span></span>
* <span data-ttu-id="d19a5-135">`ContactManagerAuthorizationHandler`: permite a los administradores aprobar o rechazar contactos.</span><span class="sxs-lookup"><span data-stu-id="d19a5-135">`ContactManagerAuthorizationHandler`: Allows managers to approve or reject contacts.</span></span>
* <span data-ttu-id="d19a5-136">`ContactAdministratorsAuthorizationHandler`: permite a los administradores aprobar o rechazar contactos y editar o eliminar contactos.</span><span class="sxs-lookup"><span data-stu-id="d19a5-136">`ContactAdministratorsAuthorizationHandler`: Allows administrators to approve or reject contacts and to edit/delete contacts.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d19a5-137">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="d19a5-137">Prerequisites</span></span>

<span data-ttu-id="d19a5-138">En este tutorial se avanza.</span><span class="sxs-lookup"><span data-stu-id="d19a5-138">This tutorial is advanced.</span></span> <span data-ttu-id="d19a5-139">Debe estar familiarizado con:</span><span class="sxs-lookup"><span data-stu-id="d19a5-139">You should be familiar with:</span></span>

* [<span data-ttu-id="d19a5-140">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d19a5-140">ASP.NET Core</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="d19a5-141">Autenticación</span><span class="sxs-lookup"><span data-stu-id="d19a5-141">Authentication</span></span>](xref:security/authentication/identity)
* [<span data-ttu-id="d19a5-142">Confirmación de cuentas y recuperación de contraseñas</span><span class="sxs-lookup"><span data-stu-id="d19a5-142">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="d19a5-143">Autorización</span><span class="sxs-lookup"><span data-stu-id="d19a5-143">Authorization</span></span>](xref:security/authorization/introduction)
* [<span data-ttu-id="d19a5-144">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="d19a5-144">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

## <a name="the-starter-and-completed-app"></a><span data-ttu-id="d19a5-145">El inicio y la aplicación completada</span><span class="sxs-lookup"><span data-stu-id="d19a5-145">The starter and completed app</span></span>

<span data-ttu-id="d19a5-146">[Descargue](xref:index#how-to-download-a-sample) la aplicación [completada](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples) .</span><span class="sxs-lookup"><span data-stu-id="d19a5-146">[Download](xref:index#how-to-download-a-sample) the [completed](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples) app.</span></span> <span data-ttu-id="d19a5-147">[Pruebe](#test-the-completed-app) la aplicación completada para familiarizarse con sus características de seguridad.</span><span class="sxs-lookup"><span data-stu-id="d19a5-147">[Test](#test-the-completed-app) the completed app so you become familiar with its security features.</span></span>

### <a name="the-starter-app"></a><span data-ttu-id="d19a5-148">La aplicación de inicio</span><span class="sxs-lookup"><span data-stu-id="d19a5-148">The starter app</span></span>

<span data-ttu-id="d19a5-149">[Descargue](xref:index#how-to-download-a-sample) la aplicación de [Inicio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/) .</span><span class="sxs-lookup"><span data-stu-id="d19a5-149">[Download](xref:index#how-to-download-a-sample) the [starter](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/) app.</span></span>

<span data-ttu-id="d19a5-150">Ejecute la aplicación, pulse el vínculo **ContactManager** y compruebe que puede crear, editar y eliminar un contacto.</span><span class="sxs-lookup"><span data-stu-id="d19a5-150">Run the app, tap the **ContactManager** link, and verify you can create, edit, and delete a contact.</span></span>

## <a name="secure-user-data"></a><span data-ttu-id="d19a5-151">Proteger los datos de usuario</span><span class="sxs-lookup"><span data-stu-id="d19a5-151">Secure user data</span></span>

<span data-ttu-id="d19a5-152">Las siguientes secciones contienen todos los pasos principales para crear la aplicación de datos de usuario segura.</span><span class="sxs-lookup"><span data-stu-id="d19a5-152">The following sections have all the major steps to create the secure user data app.</span></span> <span data-ttu-id="d19a5-153">Resultarle útil consultar el proyecto completado.</span><span class="sxs-lookup"><span data-stu-id="d19a5-153">You may find it helpful to refer to the completed project.</span></span>

### <a name="tie-the-contact-data-to-the-user"></a><span data-ttu-id="d19a5-154">Vincular los datos de contacto para el usuario</span><span class="sxs-lookup"><span data-stu-id="d19a5-154">Tie the contact data to the user</span></span>

<span data-ttu-id="d19a5-155">Use el identificador de usuario de [identidad](xref:security/authentication/identity) de ASP.net para asegurarse de que los usuarios puedan editar sus datos, pero no los datos de otros usuarios.</span><span class="sxs-lookup"><span data-stu-id="d19a5-155">Use the ASP.NET [Identity](xref:security/authentication/identity) user ID to ensure users can edit their data, but not other users data.</span></span> <span data-ttu-id="d19a5-156">Agregue `OwnerID` y `ContactStatus` al modelo de `Contact`:</span><span class="sxs-lookup"><span data-stu-id="d19a5-156">Add `OwnerID` and `ContactStatus` to the `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/final3/Models/Contact.cs?name=snippet1&highlight=5-6,16-999)]

<span data-ttu-id="d19a5-157">`OwnerID` es el identificador del usuario de la tabla de `AspNetUser` de la base de datos de [identidad](xref:security/authentication/identity) .</span><span class="sxs-lookup"><span data-stu-id="d19a5-157">`OwnerID` is the user's ID from the `AspNetUser` table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="d19a5-158">El campo `Status` determina si los usuarios generales pueden ver un contacto.</span><span class="sxs-lookup"><span data-stu-id="d19a5-158">The `Status` field determines if a contact is viewable by general users.</span></span>

<span data-ttu-id="d19a5-159">Crear una nueva migración y actualización de la base de datos:</span><span class="sxs-lookup"><span data-stu-id="d19a5-159">Create a new migration and update the database:</span></span>

```dotnetcli
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="add-role-services-to-identity"></a><span data-ttu-id="d19a5-160">Agregar servicios de función a la identidad</span><span class="sxs-lookup"><span data-stu-id="d19a5-160">Add Role services to Identity</span></span>

<span data-ttu-id="d19a5-161">Anexe [AddRoles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) para agregar servicios de rol:</span><span class="sxs-lookup"><span data-stu-id="d19a5-161">Append [AddRoles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) to add Role services:</span></span>

[!code-csharp[](secure-data/samples/final3/Startup.cs?name=snippet2&highlight=9)]

### <a name="require-authenticated-users"></a><span data-ttu-id="d19a5-162">Requerir que los usuarios autenticados</span><span class="sxs-lookup"><span data-stu-id="d19a5-162">Require authenticated users</span></span>

<span data-ttu-id="d19a5-163">Establezca la directiva de autenticación predeterminado para exigir que los usuarios se autentiquen:</span><span class="sxs-lookup"><span data-stu-id="d19a5-163">Set the default authentication policy to require users to be authenticated:</span></span>

[!code-csharp[](secure-data/samples/final3/Startup.cs?name=snippet&highlight=15-99)] 

 <span data-ttu-id="d19a5-164">Puede rechazar la autenticación en el nivel de la página de Razor, el controlador o el método de acción con el atributo `[AllowAnonymous]`.</span><span class="sxs-lookup"><span data-stu-id="d19a5-164">You can opt out of authentication at the Razor Page, controller, or action method level with the `[AllowAnonymous]` attribute.</span></span> <span data-ttu-id="d19a5-165">Configuración de la directiva de autenticación predeterminado para exigir que los usuarios se autentiquen protege recién agregada de las páginas de Razor y controladores.</span><span class="sxs-lookup"><span data-stu-id="d19a5-165">Setting the default authentication policy to require users to be authenticated protects newly added Razor Pages and controllers.</span></span> <span data-ttu-id="d19a5-166">Tener autenticación necesaria de forma predeterminada es más seguro que confiar en los nuevos controladores y Razor Pages para incluir el atributo `[Authorize]`.</span><span class="sxs-lookup"><span data-stu-id="d19a5-166">Having authentication required by default is more secure than relying on new controllers and Razor Pages to include the `[Authorize]` attribute.</span></span>

<span data-ttu-id="d19a5-167">Agregue [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) a las páginas de índice y privacidad para que los usuarios anónimos puedan obtener información sobre el sitio antes de registrarse.</span><span class="sxs-lookup"><span data-stu-id="d19a5-167">Add [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) to the Index and Privacy pages so anonymous users can get information about the site before they register.</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Index.cshtml.cs?highlight=1,7)]

### <a name="configure-the-test-account"></a><span data-ttu-id="d19a5-168">Configurar la cuenta de prueba</span><span class="sxs-lookup"><span data-stu-id="d19a5-168">Configure the test account</span></span>

<span data-ttu-id="d19a5-169">La clase `SeedData` crea dos cuentas: administrador y administrador.</span><span class="sxs-lookup"><span data-stu-id="d19a5-169">The `SeedData` class creates two accounts: administrator and manager.</span></span> <span data-ttu-id="d19a5-170">Use la [herramienta Administrador de secretos](xref:security/app-secrets) para establecer una contraseña para estas cuentas.</span><span class="sxs-lookup"><span data-stu-id="d19a5-170">Use the [Secret Manager tool](xref:security/app-secrets) to set a password for these accounts.</span></span> <span data-ttu-id="d19a5-171">Establezca la contraseña desde el directorio del proyecto (el directorio que contiene *Program.CS*):</span><span class="sxs-lookup"><span data-stu-id="d19a5-171">Set the password from the project directory (the directory containing *Program.cs*):</span></span>

```dotnetcli
dotnet user-secrets set SeedUserPW <PW>
```

<span data-ttu-id="d19a5-172">Si no se especifica una contraseña segura, se produce una excepción cuando se llama a `SeedData.Initialize`.</span><span class="sxs-lookup"><span data-stu-id="d19a5-172">If a strong password is not specified, an exception is thrown when `SeedData.Initialize` is called.</span></span>

<span data-ttu-id="d19a5-173">Actualice `Main` para usar la contraseña de prueba:</span><span class="sxs-lookup"><span data-stu-id="d19a5-173">Update `Main` to use the test password:</span></span>

[!code-csharp[](secure-data/samples/final3/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a><span data-ttu-id="d19a5-174">Crear las cuentas de prueba y actualizar los contactos</span><span class="sxs-lookup"><span data-stu-id="d19a5-174">Create the test accounts and update the contacts</span></span>

<span data-ttu-id="d19a5-175">Actualice el método `Initialize` de la clase `SeedData` para crear las cuentas de prueba:</span><span class="sxs-lookup"><span data-stu-id="d19a5-175">Update the `Initialize` method in the `SeedData` class to create the test accounts:</span></span>

[!code-csharp[](secure-data/samples/final3/Data/SeedData.cs?name=snippet_Initialize)]

<span data-ttu-id="d19a5-176">Agregue el identificador de usuario de administrador y el `ContactStatus` a los contactos.</span><span class="sxs-lookup"><span data-stu-id="d19a5-176">Add the administrator user ID and `ContactStatus` to the contacts.</span></span> <span data-ttu-id="d19a5-177">Realice uno de los contactos "Enviado" y un "rechazada".</span><span class="sxs-lookup"><span data-stu-id="d19a5-177">Make one of the contacts "Submitted" and one "Rejected".</span></span> <span data-ttu-id="d19a5-178">Agregue el Id. de usuario y el estado a todos los contactos.</span><span class="sxs-lookup"><span data-stu-id="d19a5-178">Add the user ID and status to all the contacts.</span></span> <span data-ttu-id="d19a5-179">Póngase en contacto un solo con se muestra:</span><span class="sxs-lookup"><span data-stu-id="d19a5-179">Only one contact is shown:</span></span>

[!code-csharp[](secure-data/samples/final3/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a><span data-ttu-id="d19a5-180">Crear controladores de autorización de administrador, administrador y propietario</span><span class="sxs-lookup"><span data-stu-id="d19a5-180">Create owner, manager, and administrator authorization handlers</span></span>

<span data-ttu-id="d19a5-181">Cree una clase de `ContactIsOwnerAuthorizationHandler` en la carpeta de *autorización* .</span><span class="sxs-lookup"><span data-stu-id="d19a5-181">Create a `ContactIsOwnerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="d19a5-182">En el `ContactIsOwnerAuthorizationHandler` se comprueba que el usuario que actúa en un recurso posee el recurso.</span><span class="sxs-lookup"><span data-stu-id="d19a5-182">The `ContactIsOwnerAuthorizationHandler` verifies that the user acting on a resource owns the resource.</span></span>

[!code-csharp[](secure-data/samples/final3/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

<span data-ttu-id="d19a5-183">El `ContactIsOwnerAuthorizationHandler` llama al [contexto. Se realiza correctamente](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) si el usuario autenticado actual es el propietario del contacto.</span><span class="sxs-lookup"><span data-stu-id="d19a5-183">The `ContactIsOwnerAuthorizationHandler` calls [context.Succeed](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) if the current authenticated user is the contact owner.</span></span> <span data-ttu-id="d19a5-184">Controladores de autorización con carácter general:</span><span class="sxs-lookup"><span data-stu-id="d19a5-184">Authorization handlers generally:</span></span>

* <span data-ttu-id="d19a5-185">Devuelve `context.Succeed` cuando se cumplen los requisitos.</span><span class="sxs-lookup"><span data-stu-id="d19a5-185">Return `context.Succeed` when the requirements are met.</span></span>
* <span data-ttu-id="d19a5-186">Devuelve `Task.CompletedTask` cuando no se cumplen los requisitos.</span><span class="sxs-lookup"><span data-stu-id="d19a5-186">Return `Task.CompletedTask` when requirements aren't met.</span></span> <span data-ttu-id="d19a5-187">`Task.CompletedTask` no es correcto o error&mdash;permite que se ejecuten otros controladores de autorización.</span><span class="sxs-lookup"><span data-stu-id="d19a5-187">`Task.CompletedTask` is not success or failure&mdash;it allows other authorization handlers to run.</span></span>

<span data-ttu-id="d19a5-188">Si necesita generar un error explícitamente, devuelva el [contexto. Error](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span><span class="sxs-lookup"><span data-stu-id="d19a5-188">If you need to explicitly fail, return [context.Fail](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span></span>

<span data-ttu-id="d19a5-189">La aplicación permite a los propietarios de contacto para editar, eliminar o crear sus propios datos.</span><span class="sxs-lookup"><span data-stu-id="d19a5-189">The app allows contact owners to edit/delete/create their own data.</span></span> <span data-ttu-id="d19a5-190">`ContactIsOwnerAuthorizationHandler` no necesita comprobar la operación que se ha pasado en el parámetro de requisito.</span><span class="sxs-lookup"><span data-stu-id="d19a5-190">`ContactIsOwnerAuthorizationHandler` doesn't need to check the operation passed in the requirement parameter.</span></span>

### <a name="create-a-manager-authorization-handler"></a><span data-ttu-id="d19a5-191">Crear un controlador de autorización de administrador</span><span class="sxs-lookup"><span data-stu-id="d19a5-191">Create a manager authorization handler</span></span>

<span data-ttu-id="d19a5-192">Cree una clase de `ContactManagerAuthorizationHandler` en la carpeta de *autorización* .</span><span class="sxs-lookup"><span data-stu-id="d19a5-192">Create a `ContactManagerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="d19a5-193">El `ContactManagerAuthorizationHandler` comprueba que el usuario que actúa en el recurso es un administrador.</span><span class="sxs-lookup"><span data-stu-id="d19a5-193">The `ContactManagerAuthorizationHandler` verifies the user acting on the resource is a manager.</span></span> <span data-ttu-id="d19a5-194">Solo los administradores pueden aprobar o rechazar los cambios de contenido (nuevos o modificados).</span><span class="sxs-lookup"><span data-stu-id="d19a5-194">Only managers can approve or reject content changes (new or changed).</span></span>

[!code-csharp[](secure-data/samples/final3/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a><span data-ttu-id="d19a5-195">Cree un controlador de autorización de administrador</span><span class="sxs-lookup"><span data-stu-id="d19a5-195">Create an administrator authorization handler</span></span>

<span data-ttu-id="d19a5-196">Cree una clase de `ContactAdministratorsAuthorizationHandler` en la carpeta de *autorización* .</span><span class="sxs-lookup"><span data-stu-id="d19a5-196">Create a `ContactAdministratorsAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="d19a5-197">El `ContactAdministratorsAuthorizationHandler` comprueba que el usuario que actúa en el recurso es un administrador.</span><span class="sxs-lookup"><span data-stu-id="d19a5-197">The `ContactAdministratorsAuthorizationHandler` verifies the user acting on the resource is an administrator.</span></span> <span data-ttu-id="d19a5-198">Administrador puede realizar todas las operaciones.</span><span class="sxs-lookup"><span data-stu-id="d19a5-198">Administrator can do all operations.</span></span>

[!code-csharp[](secure-data/samples/final3/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a><span data-ttu-id="d19a5-199">Registrar los controladores de autorización</span><span class="sxs-lookup"><span data-stu-id="d19a5-199">Register the authorization handlers</span></span>

<span data-ttu-id="d19a5-200">Los servicios que usan Entity Framework Core deben estar registrados para la [inserción de dependencias](xref:fundamentals/dependency-injection) mediante [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span><span class="sxs-lookup"><span data-stu-id="d19a5-200">Services using Entity Framework Core must be registered for [dependency injection](xref:fundamentals/dependency-injection) using [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span></span> <span data-ttu-id="d19a5-201">En el `ContactIsOwnerAuthorizationHandler` se usa ASP.NET Core [Identity](xref:security/authentication/identity), que se basa en Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="d19a5-201">The `ContactIsOwnerAuthorizationHandler` uses ASP.NET Core [Identity](xref:security/authentication/identity), which is built on Entity Framework Core.</span></span> <span data-ttu-id="d19a5-202">Registre los controladores con la colección de servicios para que estén disponibles para el `ContactsController` a través de la [inserción de dependencias](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="d19a5-202">Register the handlers with the service collection so they're available to the `ContactsController` through [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="d19a5-203">Agregue el código siguiente al final de `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="d19a5-203">Add the following code to the end of `ConfigureServices`:</span></span>

[!code-csharp[](secure-data/samples/final3/Startup.cs?name=snippet_defaultPolicy&highlight=23-99)]

<span data-ttu-id="d19a5-204">`ContactAdministratorsAuthorizationHandler` y `ContactManagerAuthorizationHandler` se agregan como singletons.</span><span class="sxs-lookup"><span data-stu-id="d19a5-204">`ContactAdministratorsAuthorizationHandler` and `ContactManagerAuthorizationHandler` are added as singletons.</span></span> <span data-ttu-id="d19a5-205">Son singleton porque no usan EF y toda la información necesaria está en el parámetro `Context` del método `HandleRequirementAsync`.</span><span class="sxs-lookup"><span data-stu-id="d19a5-205">They're singletons because they don't use EF and all the information needed is in the `Context` parameter of the `HandleRequirementAsync` method.</span></span>

## <a name="support-authorization"></a><span data-ttu-id="d19a5-206">Admitir la autorización</span><span class="sxs-lookup"><span data-stu-id="d19a5-206">Support authorization</span></span>

<span data-ttu-id="d19a5-207">En esta sección, actualice las páginas de Razor y agregue una clase de los requisitos de operaciones.</span><span class="sxs-lookup"><span data-stu-id="d19a5-207">In this section, you update the Razor Pages and add an operations requirements class.</span></span>

### <a name="review-the-contact-operations-requirements-class"></a><span data-ttu-id="d19a5-208">Revisión de la clase de los requisitos de las operaciones de contacto</span><span class="sxs-lookup"><span data-stu-id="d19a5-208">Review the contact operations requirements class</span></span>

<span data-ttu-id="d19a5-209">Revise la clase `ContactOperations`.</span><span class="sxs-lookup"><span data-stu-id="d19a5-209">Review the `ContactOperations` class.</span></span> <span data-ttu-id="d19a5-210">Esta clase contiene los requisitos que admite la aplicación:</span><span class="sxs-lookup"><span data-stu-id="d19a5-210">This class contains the requirements the app supports:</span></span>

[!code-csharp[](secure-data/samples/final3/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-contacts-razor-pages"></a><span data-ttu-id="d19a5-211">Crear una clase base para las páginas de Razor de contactos</span><span class="sxs-lookup"><span data-stu-id="d19a5-211">Create a base class for the Contacts Razor Pages</span></span>

<span data-ttu-id="d19a5-212">Cree una clase base que contiene los servicios usados en los contactos de las páginas de Razor.</span><span class="sxs-lookup"><span data-stu-id="d19a5-212">Create a base class that contains the services used in the contacts Razor Pages.</span></span> <span data-ttu-id="d19a5-213">La clase base coloca el código de inicialización en una ubicación:</span><span class="sxs-lookup"><span data-stu-id="d19a5-213">The base class puts the initialization code in one location:</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/DI_BasePageModel.cs)]

<span data-ttu-id="d19a5-214">El código anterior:</span><span class="sxs-lookup"><span data-stu-id="d19a5-214">The preceding code:</span></span>

* <span data-ttu-id="d19a5-215">Agrega el servicio `IAuthorizationService` para tener acceso a los controladores de autorización.</span><span class="sxs-lookup"><span data-stu-id="d19a5-215">Adds the `IAuthorizationService` service to access to the authorization handlers.</span></span>
* <span data-ttu-id="d19a5-216">Agrega el servicio Identity `UserManager`.</span><span class="sxs-lookup"><span data-stu-id="d19a5-216">Adds the Identity `UserManager` service.</span></span>
* <span data-ttu-id="d19a5-217">Agregue la `ApplicationDbContext`.</span><span class="sxs-lookup"><span data-stu-id="d19a5-217">Add the `ApplicationDbContext`.</span></span>

### <a name="update-the-createmodel"></a><span data-ttu-id="d19a5-218">Actualizar el CreateModel</span><span class="sxs-lookup"><span data-stu-id="d19a5-218">Update the CreateModel</span></span>

<span data-ttu-id="d19a5-219">Actualice el constructor Create Page Model para usar la clase base `DI_BasePageModel`:</span><span class="sxs-lookup"><span data-stu-id="d19a5-219">Update the create page model constructor to use the `DI_BasePageModel` base class:</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

<span data-ttu-id="d19a5-220">Actualice el método `CreateModel.OnPostAsync` para:</span><span class="sxs-lookup"><span data-stu-id="d19a5-220">Update the `CreateModel.OnPostAsync` method to:</span></span>

* <span data-ttu-id="d19a5-221">Agregue el identificador de usuario al modelo de `Contact`.</span><span class="sxs-lookup"><span data-stu-id="d19a5-221">Add the user ID to the `Contact` model.</span></span>
* <span data-ttu-id="d19a5-222">Llamar al controlador de autorización para comprobar que el usuario tiene permiso para crear contactos.</span><span class="sxs-lookup"><span data-stu-id="d19a5-222">Call the authorization handler to verify the user has permission to create contacts.</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a><span data-ttu-id="d19a5-223">Actualizar el IndexModel</span><span class="sxs-lookup"><span data-stu-id="d19a5-223">Update the IndexModel</span></span>

<span data-ttu-id="d19a5-224">Actualice el método de `OnGetAsync` de modo que solo se muestren los contactos aprobados a los usuarios generales:</span><span class="sxs-lookup"><span data-stu-id="d19a5-224">Update the `OnGetAsync` method so only approved contacts are shown to general users:</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a><span data-ttu-id="d19a5-225">Actualizar el EditModel</span><span class="sxs-lookup"><span data-stu-id="d19a5-225">Update the EditModel</span></span>

<span data-ttu-id="d19a5-226">Agregar un controlador de autorización para comprobar que el usuario propietario del contacto.</span><span class="sxs-lookup"><span data-stu-id="d19a5-226">Add an authorization handler to verify the user owns the contact.</span></span> <span data-ttu-id="d19a5-227">Dado que se está validando la autorización de recursos, el atributo `[Authorize]` no es suficiente.</span><span class="sxs-lookup"><span data-stu-id="d19a5-227">Because resource authorization is being validated, the `[Authorize]` attribute is not enough.</span></span> <span data-ttu-id="d19a5-228">La aplicación no tiene acceso al recurso cuando se evalúan los atributos.</span><span class="sxs-lookup"><span data-stu-id="d19a5-228">The app doesn't have access to the resource when attributes are evaluated.</span></span> <span data-ttu-id="d19a5-229">Autorización basada en el recurso debe ser un imperativo.</span><span class="sxs-lookup"><span data-stu-id="d19a5-229">Resource-based authorization must be imperative.</span></span> <span data-ttu-id="d19a5-230">Las comprobaciones deben realizarse una vez que la aplicación tenga acceso al recurso, cargándolo en el modelo de página o cargándola en el controlador de sí mismo.</span><span class="sxs-lookup"><span data-stu-id="d19a5-230">Checks must be performed once the app has access to the resource, either by loading it in the page model or by loading it within the handler itself.</span></span> <span data-ttu-id="d19a5-231">Con frecuencia acceder al recurso pasando la clave de recurso.</span><span class="sxs-lookup"><span data-stu-id="d19a5-231">You frequently access the resource by passing in the resource key.</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a><span data-ttu-id="d19a5-232">Actualizar el DeleteModel</span><span class="sxs-lookup"><span data-stu-id="d19a5-232">Update the DeleteModel</span></span>

<span data-ttu-id="d19a5-233">Actualice el modelo de página de delete para usar el controlador de autorización para comprobar que el usuario tiene permiso delete en el contacto.</span><span class="sxs-lookup"><span data-stu-id="d19a5-233">Update the delete page model to use the authorization handler to verify the user has delete permission on the contact.</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a><span data-ttu-id="d19a5-234">Insertar el servicio de autorización en las vistas</span><span class="sxs-lookup"><span data-stu-id="d19a5-234">Inject the authorization service into the views</span></span>

<span data-ttu-id="d19a5-235">Actualmente, se muestra en la interfaz de usuario edita y elimina los vínculos para los contactos que no se puede modificar el usuario.</span><span class="sxs-lookup"><span data-stu-id="d19a5-235">Currently, the UI shows edit and delete links for contacts the user can't modify.</span></span>

<span data-ttu-id="d19a5-236">Inserte el servicio de autorización en el archivo *pages/_ViewImports. cshtml* para que esté disponible para todas las vistas:</span><span class="sxs-lookup"><span data-stu-id="d19a5-236">Inject the authorization service in the *Pages/_ViewImports.cshtml* file so it's available to all views:</span></span>

[!code-cshtml[](secure-data/samples/final3/Pages/_ViewImports.cshtml?highlight=6-99)]

<span data-ttu-id="d19a5-237">El marcado anterior agrega varias instrucciones de `using`.</span><span class="sxs-lookup"><span data-stu-id="d19a5-237">The preceding markup adds several `using` statements.</span></span>

<span data-ttu-id="d19a5-238">Actualice los vínculos de **edición** y **eliminación** en *pages/Contacts/index. cshtml* para que solo se representen para los usuarios con los permisos adecuados:</span><span class="sxs-lookup"><span data-stu-id="d19a5-238">Update the **Edit** and **Delete** links in *Pages/Contacts/Index.cshtml* so they're only rendered for users with the appropriate permissions:</span></span>

[!code-cshtml[](secure-data/samples/final3/Pages/Contacts/Index.cshtml?highlight=34-36,62-999)]

> [!WARNING]
> <span data-ttu-id="d19a5-239">Ocultar los vínculos de los usuarios que no tienen permiso para cambiar los datos no proteger la aplicación.</span><span class="sxs-lookup"><span data-stu-id="d19a5-239">Hiding links from users that don't have permission to change data doesn't secure the app.</span></span> <span data-ttu-id="d19a5-240">Ocultar vínculos hace que la aplicación más fácil de usar mostrando vínculos solo es válido.</span><span class="sxs-lookup"><span data-stu-id="d19a5-240">Hiding links makes the app more user-friendly by displaying only valid links.</span></span> <span data-ttu-id="d19a5-241">Los usuarios pueden hack las direcciones URL generadas para invocar Editar y eliminar operaciones en los datos que no poseen.</span><span class="sxs-lookup"><span data-stu-id="d19a5-241">Users can hack the generated URLs to invoke edit and delete operations on data they don't own.</span></span> <span data-ttu-id="d19a5-242">La página de Razor o el controlador debe exigir comprobaciones de acceso para proteger los datos.</span><span class="sxs-lookup"><span data-stu-id="d19a5-242">The Razor Page or controller must enforce access checks to secure the data.</span></span>

### <a name="update-details"></a><span data-ttu-id="d19a5-243">Detalles de la actualización</span><span class="sxs-lookup"><span data-stu-id="d19a5-243">Update Details</span></span>

<span data-ttu-id="d19a5-244">Actualice la vista de detalles para que los administradores pueden aprobar o rechazar contactos:</span><span class="sxs-lookup"><span data-stu-id="d19a5-244">Update the details view so managers can approve or reject contacts:</span></span>

[!code-cshtml[](secure-data/samples/final3/Pages/Contacts/Details.cshtml?name=snippet)]

<span data-ttu-id="d19a5-245">Actualice el modelo de página de detalles:</span><span class="sxs-lookup"><span data-stu-id="d19a5-245">Update the details page model:</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="add-or-remove-a-user-to-a-role"></a><span data-ttu-id="d19a5-246">Agregar o quitar un usuario a un rol</span><span class="sxs-lookup"><span data-stu-id="d19a5-246">Add or remove a user to a role</span></span>

<span data-ttu-id="d19a5-247">Consulte [este problema](https://github.com/aspnet/AspNetCore.Docs/issues/8502) para obtener información sobre:</span><span class="sxs-lookup"><span data-stu-id="d19a5-247">See [this issue](https://github.com/aspnet/AspNetCore.Docs/issues/8502) for information on:</span></span>

* <span data-ttu-id="d19a5-248">Quita los privilegios de un usuario.</span><span class="sxs-lookup"><span data-stu-id="d19a5-248">Removing privileges from a user.</span></span> <span data-ttu-id="d19a5-249">Por ejemplo, silenciar a un usuario en una aplicación de chat.</span><span class="sxs-lookup"><span data-stu-id="d19a5-249">For example, muting a user in a chat app.</span></span>
* <span data-ttu-id="d19a5-250">Agregar privilegios a un usuario.</span><span class="sxs-lookup"><span data-stu-id="d19a5-250">Adding privileges to a user.</span></span>

<a name="challenge"></a>

## <a name="differences-between-challenge-and-forbid"></a><span data-ttu-id="d19a5-251">Diferencias entre desafío y prohibido</span><span class="sxs-lookup"><span data-stu-id="d19a5-251">Differences between Challenge and Forbid</span></span>

<span data-ttu-id="d19a5-252">Esta aplicación establece la directiva predeterminada para [requerir usuarios autenticados](#require-authenticated-users).</span><span class="sxs-lookup"><span data-stu-id="d19a5-252">This app sets the default policy to [require authenticated users](#require-authenticated-users).</span></span> <span data-ttu-id="d19a5-253">El código siguiente permite usuarios anónimos.</span><span class="sxs-lookup"><span data-stu-id="d19a5-253">The following code allows anonymous users.</span></span> <span data-ttu-id="d19a5-254">Los usuarios anónimos pueden mostrar las diferencias entre desafío y prohibido.</span><span class="sxs-lookup"><span data-stu-id="d19a5-254">Anonymous users are allowed to show the differences between Challenge vs Forbid.</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Details2.cshtml.cs?name=snippet)]

<span data-ttu-id="d19a5-255">En el código anterior:</span><span class="sxs-lookup"><span data-stu-id="d19a5-255">In the preceding code:</span></span>

* <span data-ttu-id="d19a5-256">Cuando el usuario **no** está autenticado, se devuelve un `ChallengeResult`.</span><span class="sxs-lookup"><span data-stu-id="d19a5-256">When the user is **not** authenticated, a `ChallengeResult` is returned.</span></span> <span data-ttu-id="d19a5-257">Cuando se devuelve un `ChallengeResult`, se redirige al usuario a la página de inicio de sesión.</span><span class="sxs-lookup"><span data-stu-id="d19a5-257">When a `ChallengeResult` is returned, the user is redirected to the sign-in page.</span></span>
* <span data-ttu-id="d19a5-258">Cuando el usuario está autenticado, pero no autorizado, se devuelve un `ForbidResult`.</span><span class="sxs-lookup"><span data-stu-id="d19a5-258">When the user is authenticated, but not authorized, a `ForbidResult` is returned.</span></span> <span data-ttu-id="d19a5-259">Cuando se devuelve un `ForbidResult`, se redirige al usuario a la página acceso denegado.</span><span class="sxs-lookup"><span data-stu-id="d19a5-259">When a `ForbidResult` is returned, the user is redirected to the access denied page.</span></span>

## <a name="test-the-completed-app"></a><span data-ttu-id="d19a5-260">Probar la aplicación completada</span><span class="sxs-lookup"><span data-stu-id="d19a5-260">Test the completed app</span></span>

<span data-ttu-id="d19a5-261">Si aún no ha establecido una contraseña para las cuentas de usuario inicializadas, use la [herramienta Administrador de secretos](xref:security/app-secrets#secret-manager) para establecer una contraseña:</span><span class="sxs-lookup"><span data-stu-id="d19a5-261">If you haven't already set a password for seeded user accounts, use the [Secret Manager tool](xref:security/app-secrets#secret-manager) to set a password:</span></span>

* <span data-ttu-id="d19a5-262">Elija una contraseña segura: Use ocho o más caracteres y al menos una mayúscula, número y símbolos.</span><span class="sxs-lookup"><span data-stu-id="d19a5-262">Choose a strong password: Use eight or more characters and at least one upper-case character, number, and symbol.</span></span> <span data-ttu-id="d19a5-263">Por ejemplo, `Passw0rd!` cumple los requisitos de contraseñas seguras.</span><span class="sxs-lookup"><span data-stu-id="d19a5-263">For example, `Passw0rd!` meets the strong password requirements.</span></span>
* <span data-ttu-id="d19a5-264">Ejecute el siguiente comando desde la carpeta del proyecto, donde `<PW>` es la contraseña:</span><span class="sxs-lookup"><span data-stu-id="d19a5-264">Execute the following command from the project's folder, where `<PW>` is the password:</span></span>

  ```dotnetcli
  dotnet user-secrets set SeedUserPW <PW>
  ```

<span data-ttu-id="d19a5-265">Si la aplicación tiene contactos:</span><span class="sxs-lookup"><span data-stu-id="d19a5-265">If the app has contacts:</span></span>

* <span data-ttu-id="d19a5-266">Elimine todos los registros de la tabla `Contact`.</span><span class="sxs-lookup"><span data-stu-id="d19a5-266">Delete all of the records in the `Contact` table.</span></span>
* <span data-ttu-id="d19a5-267">Reinicie la aplicación para inicializar la base de datos.</span><span class="sxs-lookup"><span data-stu-id="d19a5-267">Restart the app to seed the database.</span></span>

<span data-ttu-id="d19a5-268">Es una manera fácil de probar la aplicación completa iniciar los tres distintos exploradores (o las sesiones de InPrivate o incognito).</span><span class="sxs-lookup"><span data-stu-id="d19a5-268">An easy way to test the completed app is to launch three different browsers (or incognito/InPrivate sessions).</span></span> <span data-ttu-id="d19a5-269">En un explorador, registre un nuevo usuario (por ejemplo, `test@example.com`).</span><span class="sxs-lookup"><span data-stu-id="d19a5-269">In one browser, register a new user (for example, `test@example.com`).</span></span> <span data-ttu-id="d19a5-270">Inicie sesión con un usuario diferente en cada explorador.</span><span class="sxs-lookup"><span data-stu-id="d19a5-270">Sign in to each browser with a different user.</span></span> <span data-ttu-id="d19a5-271">Compruebe las siguientes operaciones:</span><span class="sxs-lookup"><span data-stu-id="d19a5-271">Verify the following operations:</span></span>

* <span data-ttu-id="d19a5-272">Usuarios registrados pueden ver todos los datos de contacto aprobados.</span><span class="sxs-lookup"><span data-stu-id="d19a5-272">Registered users can view all of the approved contact data.</span></span>
* <span data-ttu-id="d19a5-273">Los usuarios registrados pueden editar o eliminar sus propios datos.</span><span class="sxs-lookup"><span data-stu-id="d19a5-273">Registered users can edit/delete their own data.</span></span>
* <span data-ttu-id="d19a5-274">Los administradores pueden aprobar o rechazar datos de contacto.</span><span class="sxs-lookup"><span data-stu-id="d19a5-274">Managers can approve/reject contact data.</span></span> <span data-ttu-id="d19a5-275">En la vista `Details` se muestran los botones **aprobar** y **rechazar** .</span><span class="sxs-lookup"><span data-stu-id="d19a5-275">The `Details` view shows **Approve** and **Reject** buttons.</span></span>
* <span data-ttu-id="d19a5-276">Los administradores pueden aprobar o rechazar y editar o eliminar todos los datos.</span><span class="sxs-lookup"><span data-stu-id="d19a5-276">Administrators can approve/reject and edit/delete all data.</span></span>

| <span data-ttu-id="d19a5-277">Usuario</span><span class="sxs-lookup"><span data-stu-id="d19a5-277">User</span></span>                | <span data-ttu-id="d19a5-278">Propagadas por la aplicación</span><span class="sxs-lookup"><span data-stu-id="d19a5-278">Seeded by the app</span></span> | <span data-ttu-id="d19a5-279">Opciones</span><span class="sxs-lookup"><span data-stu-id="d19a5-279">Options</span></span>                                  |
| ------------------- | :---------------: | ---------------------------------------- |
| test@example.com    | <span data-ttu-id="d19a5-280">No</span><span class="sxs-lookup"><span data-stu-id="d19a5-280">No</span></span>                | <span data-ttu-id="d19a5-281">Editar o eliminar los datos propios.</span><span class="sxs-lookup"><span data-stu-id="d19a5-281">Edit/delete the own data.</span></span>                |
| manager@contoso.com | <span data-ttu-id="d19a5-282">Sí</span><span class="sxs-lookup"><span data-stu-id="d19a5-282">Yes</span></span>               | <span data-ttu-id="d19a5-283">Aprobar o rechazar y editar o eliminar los datos propios.</span><span class="sxs-lookup"><span data-stu-id="d19a5-283">Approve/reject and edit/delete own data.</span></span> |
| admin@contoso.com   | <span data-ttu-id="d19a5-284">Sí</span><span class="sxs-lookup"><span data-stu-id="d19a5-284">Yes</span></span>               | <span data-ttu-id="d19a5-285">Aprobar o rechazar y editar o eliminar todos los datos.</span><span class="sxs-lookup"><span data-stu-id="d19a5-285">Approve/reject and edit/delete all data.</span></span> |

<span data-ttu-id="d19a5-286">Crear un contacto en el explorador del administrador.</span><span class="sxs-lookup"><span data-stu-id="d19a5-286">Create a contact in the administrator's browser.</span></span> <span data-ttu-id="d19a5-287">Copie la dirección URL para su eliminación y editar en el contacto del administrador.</span><span class="sxs-lookup"><span data-stu-id="d19a5-287">Copy the URL for delete and edit from the administrator contact.</span></span> <span data-ttu-id="d19a5-288">Pegue estos vínculos en el explorador del usuario de prueba para comprobar que el usuario de prueba no puede realizar estas operaciones.</span><span class="sxs-lookup"><span data-stu-id="d19a5-288">Paste these links into the test user's browser to verify the test user can't perform these operations.</span></span>

## <a name="create-the-starter-app"></a><span data-ttu-id="d19a5-289">Crear la aplicación de inicio</span><span class="sxs-lookup"><span data-stu-id="d19a5-289">Create the starter app</span></span>

* <span data-ttu-id="d19a5-290">Cree una aplicación de páginas de Razor denominada "ContactManager"</span><span class="sxs-lookup"><span data-stu-id="d19a5-290">Create a Razor Pages app named "ContactManager"</span></span>
  * <span data-ttu-id="d19a5-291">Cree la aplicación con **cuentas de usuario individuales**.</span><span class="sxs-lookup"><span data-stu-id="d19a5-291">Create the app with **Individual User Accounts**.</span></span>
  * <span data-ttu-id="d19a5-292">Asígnele el nombre "ContactManager" para el espacio de nombres coincida con el espacio de nombres utilizado en el ejemplo.</span><span class="sxs-lookup"><span data-stu-id="d19a5-292">Name it "ContactManager" so the namespace matches the namespace used in the sample.</span></span>
  * <span data-ttu-id="d19a5-293">`-uld` especifica LocalDB en lugar de SQLite</span><span class="sxs-lookup"><span data-stu-id="d19a5-293">`-uld` specifies LocalDB instead of SQLite</span></span>

  ```dotnetcli
  dotnet new webapp -o ContactManager -au Individual -uld
  ```

* <span data-ttu-id="d19a5-294">Agregue *Models/contact. CS*:</span><span class="sxs-lookup"><span data-stu-id="d19a5-294">Add *Models/Contact.cs*:</span></span>

  [!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

* <span data-ttu-id="d19a5-295">Aplicar scaffolding al modelo de `Contact`.</span><span class="sxs-lookup"><span data-stu-id="d19a5-295">Scaffold the `Contact` model.</span></span>
* <span data-ttu-id="d19a5-296">Crear la migración inicial y actualizar la base de datos:</span><span class="sxs-lookup"><span data-stu-id="d19a5-296">Create initial migration and update the database:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet tool install -g dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
dotnet ef database drop -f
dotnet ef migrations add initial
dotnet ef database update
```

<span data-ttu-id="d19a5-297">Si experimenta un error con el comando `dotnet aspnet-codegenerator razorpage`, consulte [este problema de github](https://github.com/aspnet/Scaffolding/issues/984).</span><span class="sxs-lookup"><span data-stu-id="d19a5-297">If you experience a bug with the `dotnet aspnet-codegenerator razorpage` command, see [this GitHub issue](https://github.com/aspnet/Scaffolding/issues/984).</span></span>

* <span data-ttu-id="d19a5-298">Actualice el delimitador **ContactManager** en el archivo *pages/Shared/_Layout. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="d19a5-298">Update the **ContactManager** anchor in the *Pages/Shared/_Layout.cshtml* file:</span></span>

 ```cshtml
<a class="navbar-brand" asp-area="" asp-page="/Contacts/Index">ContactManager</a>
  ```

* <span data-ttu-id="d19a5-299">Probar la aplicación mediante la creación, edición y eliminación de un contacto</span><span class="sxs-lookup"><span data-stu-id="d19a5-299">Test the app by creating, editing, and deleting a contact</span></span>

### <a name="seed-the-database"></a><span data-ttu-id="d19a5-300">Inicializar la base de datos</span><span class="sxs-lookup"><span data-stu-id="d19a5-300">Seed the database</span></span>

<span data-ttu-id="d19a5-301">Agregue la clase [SeedData](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter3/Data/SeedData.cs) a la carpeta de *datos* :</span><span class="sxs-lookup"><span data-stu-id="d19a5-301">Add the [SeedData](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter3/Data/SeedData.cs) class to the *Data* folder:</span></span>

[!code-csharp[](secure-data/samples/starter3/Data/SeedData.cs)]

<span data-ttu-id="d19a5-302">Llame a `SeedData.Initialize` desde `Main`:</span><span class="sxs-lookup"><span data-stu-id="d19a5-302">Call `SeedData.Initialize` from `Main`:</span></span>

[!code-csharp[](secure-data/samples/starter3/Program.cs)]

<span data-ttu-id="d19a5-303">Probar que la aplicación había propagado la base de datos.</span><span class="sxs-lookup"><span data-stu-id="d19a5-303">Test that the app seeded the database.</span></span> <span data-ttu-id="d19a5-304">Si no hay ninguna fila en la base de datos de contacto, no se ejecuta el método de inicialización.</span><span class="sxs-lookup"><span data-stu-id="d19a5-304">If there are any rows in the contact DB, the seed method doesn't run.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1 < aspnetcore-3.0"

<span data-ttu-id="d19a5-305">Este tutorial muestra cómo crear una aplicación web ASP.NET Core con los datos protegidos por autorización del usuario.</span><span class="sxs-lookup"><span data-stu-id="d19a5-305">This tutorial shows how to create an ASP.NET Core web app with user data protected by authorization.</span></span> <span data-ttu-id="d19a5-306">Muestra una lista de contactos que autentican los usuarios (registrados) ha creado.</span><span class="sxs-lookup"><span data-stu-id="d19a5-306">It displays a list of contacts that authenticated (registered) users have created.</span></span> <span data-ttu-id="d19a5-307">Hay tres grupos de seguridad:</span><span class="sxs-lookup"><span data-stu-id="d19a5-307">There are three security groups:</span></span>

* <span data-ttu-id="d19a5-308">**Los usuarios registrados** pueden ver todos los datos aprobados y pueden editar o eliminar sus propios datos.</span><span class="sxs-lookup"><span data-stu-id="d19a5-308">**Registered users** can view all the approved data and can edit/delete their own data.</span></span>
* <span data-ttu-id="d19a5-309">Los **administradores** pueden aprobar o rechazar datos de contacto.</span><span class="sxs-lookup"><span data-stu-id="d19a5-309">**Managers** can approve or reject contact data.</span></span> <span data-ttu-id="d19a5-310">Solo contactos aprobados son visibles para los usuarios.</span><span class="sxs-lookup"><span data-stu-id="d19a5-310">Only approved contacts are visible to users.</span></span>
* <span data-ttu-id="d19a5-311">**Los administradores** pueden aprobar o rechazar y editar o eliminar los datos.</span><span class="sxs-lookup"><span data-stu-id="d19a5-311">**Administrators** can approve/reject and edit/delete any data.</span></span>

<span data-ttu-id="d19a5-312">En la siguiente imagen, el usuario Rick (`rick@example.com`) ha iniciado sesión.</span><span class="sxs-lookup"><span data-stu-id="d19a5-312">In the following image, user Rick (`rick@example.com`) is signed in.</span></span> <span data-ttu-id="d19a5-313">Rick solo puede ver contactos aprobados y **editar**/**eliminar**/**crear nuevos** vínculos para sus contactos.</span><span class="sxs-lookup"><span data-stu-id="d19a5-313">Rick can only view approved contacts and **Edit**/**Delete**/**Create New** links for his contacts.</span></span> <span data-ttu-id="d19a5-314">Solo el último registro, creado por Rick, muestra los vínculos de **edición** y **eliminación** .</span><span class="sxs-lookup"><span data-stu-id="d19a5-314">Only the last record, created by Rick, displays **Edit** and **Delete** links.</span></span> <span data-ttu-id="d19a5-315">Otros usuarios no verán el último registro hasta un administrador o un administrador cambia el estado a "Aprobado".</span><span class="sxs-lookup"><span data-stu-id="d19a5-315">Other users won't see the last record until a manager or administrator changes the status to "Approved".</span></span>

![Captura de pantalla con Rick ha iniciado sesión](secure-data/_static/rick.png)

<span data-ttu-id="d19a5-317">En la imagen siguiente, `manager@contoso.com` está conectado y en el rol del administrador:</span><span class="sxs-lookup"><span data-stu-id="d19a5-317">In the following image, `manager@contoso.com` is signed in and in the manager's role:</span></span>

![Captura de pantalla que muestra manager@contoso.com sesión iniciada](secure-data/_static/manager1.png)

<span data-ttu-id="d19a5-319">La siguiente imagen muestra a los administradores de la vista de detalles de un contacto:</span><span class="sxs-lookup"><span data-stu-id="d19a5-319">The following image shows the managers details view of a contact:</span></span>

![Vista del Administrador de un contacto](secure-data/_static/manager.png)

<span data-ttu-id="d19a5-321">Los botones **aprobar** y **rechazar** solo se muestran para administradores y administradores.</span><span class="sxs-lookup"><span data-stu-id="d19a5-321">The **Approve** and **Reject** buttons are only displayed for managers and administrators.</span></span>

<span data-ttu-id="d19a5-322">En la siguiente imagen, `admin@contoso.com` está conectado y en el rol de administrador:</span><span class="sxs-lookup"><span data-stu-id="d19a5-322">In the following image, `admin@contoso.com` is signed in and in the administrator's role:</span></span>

![Captura de pantalla que muestra admin@contoso.com sesión iniciada](secure-data/_static/admin.png)

<span data-ttu-id="d19a5-324">El administrador tiene todos los privilegios.</span><span class="sxs-lookup"><span data-stu-id="d19a5-324">The administrator has all privileges.</span></span> <span data-ttu-id="d19a5-325">Puede leer, editar o eliminar cualquier contacto y cambiar el estado de los contactos.</span><span class="sxs-lookup"><span data-stu-id="d19a5-325">She can read/edit/delete any contact and change the status of contacts.</span></span>

<span data-ttu-id="d19a5-326">La aplicación se creó mediante [scaffolding](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model) en el siguiente modelo de `Contact`:</span><span class="sxs-lookup"><span data-stu-id="d19a5-326">The app was created by [scaffolding](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model) the following `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

<span data-ttu-id="d19a5-327">El ejemplo contiene los siguientes controladores de autorización:</span><span class="sxs-lookup"><span data-stu-id="d19a5-327">The sample contains the following authorization handlers:</span></span>

* <span data-ttu-id="d19a5-328">`ContactIsOwnerAuthorizationHandler`: garantiza que un usuario solo pueda editar sus datos.</span><span class="sxs-lookup"><span data-stu-id="d19a5-328">`ContactIsOwnerAuthorizationHandler`: Ensures that a user can only edit their data.</span></span>
* <span data-ttu-id="d19a5-329">`ContactManagerAuthorizationHandler`: permite a los administradores aprobar o rechazar contactos.</span><span class="sxs-lookup"><span data-stu-id="d19a5-329">`ContactManagerAuthorizationHandler`: Allows managers to approve or reject contacts.</span></span>
* <span data-ttu-id="d19a5-330">`ContactAdministratorsAuthorizationHandler`: permite a los administradores aprobar o rechazar contactos y editar o eliminar contactos.</span><span class="sxs-lookup"><span data-stu-id="d19a5-330">`ContactAdministratorsAuthorizationHandler`: Allows administrators to approve or reject contacts and to edit/delete contacts.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d19a5-331">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="d19a5-331">Prerequisites</span></span>

<span data-ttu-id="d19a5-332">En este tutorial se avanza.</span><span class="sxs-lookup"><span data-stu-id="d19a5-332">This tutorial is advanced.</span></span> <span data-ttu-id="d19a5-333">Debe estar familiarizado con:</span><span class="sxs-lookup"><span data-stu-id="d19a5-333">You should be familiar with:</span></span>

* [<span data-ttu-id="d19a5-334">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d19a5-334">ASP.NET Core</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="d19a5-335">Autenticación</span><span class="sxs-lookup"><span data-stu-id="d19a5-335">Authentication</span></span>](xref:security/authentication/identity)
* [<span data-ttu-id="d19a5-336">Confirmación de cuentas y recuperación de contraseñas</span><span class="sxs-lookup"><span data-stu-id="d19a5-336">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="d19a5-337">Autorización</span><span class="sxs-lookup"><span data-stu-id="d19a5-337">Authorization</span></span>](xref:security/authorization/introduction)
* [<span data-ttu-id="d19a5-338">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="d19a5-338">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

## <a name="the-starter-and-completed-app"></a><span data-ttu-id="d19a5-339">El inicio y la aplicación completada</span><span class="sxs-lookup"><span data-stu-id="d19a5-339">The starter and completed app</span></span>

<span data-ttu-id="d19a5-340">[Descargue](xref:index#how-to-download-a-sample) la aplicación [completada](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples) .</span><span class="sxs-lookup"><span data-stu-id="d19a5-340">[Download](xref:index#how-to-download-a-sample) the [completed](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples) app.</span></span> <span data-ttu-id="d19a5-341">[Pruebe](#test-the-completed-app) la aplicación completada para familiarizarse con sus características de seguridad.</span><span class="sxs-lookup"><span data-stu-id="d19a5-341">[Test](#test-the-completed-app) the completed app so you become familiar with its security features.</span></span>

### <a name="the-starter-app"></a><span data-ttu-id="d19a5-342">La aplicación de inicio</span><span class="sxs-lookup"><span data-stu-id="d19a5-342">The starter app</span></span>

<span data-ttu-id="d19a5-343">[Descargue](xref:index#how-to-download-a-sample) la aplicación de [Inicio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/) .</span><span class="sxs-lookup"><span data-stu-id="d19a5-343">[Download](xref:index#how-to-download-a-sample) the [starter](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/) app.</span></span>

<span data-ttu-id="d19a5-344">Ejecute la aplicación, pulse el vínculo **ContactManager** y compruebe que puede crear, editar y eliminar un contacto.</span><span class="sxs-lookup"><span data-stu-id="d19a5-344">Run the app, tap the **ContactManager** link, and verify you can create, edit, and delete a contact.</span></span>

## <a name="secure-user-data"></a><span data-ttu-id="d19a5-345">Proteger los datos de usuario</span><span class="sxs-lookup"><span data-stu-id="d19a5-345">Secure user data</span></span>

<span data-ttu-id="d19a5-346">Las siguientes secciones contienen todos los pasos principales para crear la aplicación de datos de usuario segura.</span><span class="sxs-lookup"><span data-stu-id="d19a5-346">The following sections have all the major steps to create the secure user data app.</span></span> <span data-ttu-id="d19a5-347">Resultarle útil consultar el proyecto completado.</span><span class="sxs-lookup"><span data-stu-id="d19a5-347">You may find it helpful to refer to the completed project.</span></span>

### <a name="tie-the-contact-data-to-the-user"></a><span data-ttu-id="d19a5-348">Vincular los datos de contacto para el usuario</span><span class="sxs-lookup"><span data-stu-id="d19a5-348">Tie the contact data to the user</span></span>

<span data-ttu-id="d19a5-349">Use el identificador de usuario de [identidad](xref:security/authentication/identity) de ASP.net para asegurarse de que los usuarios puedan editar sus datos, pero no los datos de otros usuarios.</span><span class="sxs-lookup"><span data-stu-id="d19a5-349">Use the ASP.NET [Identity](xref:security/authentication/identity) user ID to ensure users can edit their data, but not other users data.</span></span> <span data-ttu-id="d19a5-350">Agregue `OwnerID` y `ContactStatus` al modelo de `Contact`:</span><span class="sxs-lookup"><span data-stu-id="d19a5-350">Add `OwnerID` and `ContactStatus` to the `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Models/Contact.cs?name=snippet1&highlight=5-6,16-999)]

<span data-ttu-id="d19a5-351">`OwnerID` es el identificador del usuario de la tabla de `AspNetUser` de la base de datos de [identidad](xref:security/authentication/identity) .</span><span class="sxs-lookup"><span data-stu-id="d19a5-351">`OwnerID` is the user's ID from the `AspNetUser` table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="d19a5-352">El campo `Status` determina si los usuarios generales pueden ver un contacto.</span><span class="sxs-lookup"><span data-stu-id="d19a5-352">The `Status` field determines if a contact is viewable by general users.</span></span>

<span data-ttu-id="d19a5-353">Crear una nueva migración y actualización de la base de datos:</span><span class="sxs-lookup"><span data-stu-id="d19a5-353">Create a new migration and update the database:</span></span>

```dotnetcli
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="add-role-services-to-identity"></a><span data-ttu-id="d19a5-354">Agregar servicios de función a la identidad</span><span class="sxs-lookup"><span data-stu-id="d19a5-354">Add Role services to Identity</span></span>

<span data-ttu-id="d19a5-355">Anexe [AddRoles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) para agregar servicios de rol:</span><span class="sxs-lookup"><span data-stu-id="d19a5-355">Append [AddRoles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) to add Role services:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet2&highlight=12)]

### <a name="require-authenticated-users"></a><span data-ttu-id="d19a5-356">Requerir que los usuarios autenticados</span><span class="sxs-lookup"><span data-stu-id="d19a5-356">Require authenticated users</span></span>

<span data-ttu-id="d19a5-357">Establezca la directiva de autenticación predeterminado para exigir que los usuarios se autentiquen:</span><span class="sxs-lookup"><span data-stu-id="d19a5-357">Set the default authentication policy to require users to be authenticated:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet&highlight=17-99)] 

 <span data-ttu-id="d19a5-358">Puede rechazar la autenticación en el nivel de la página de Razor, el controlador o el método de acción con el atributo `[AllowAnonymous]`.</span><span class="sxs-lookup"><span data-stu-id="d19a5-358">You can opt out of authentication at the Razor Page, controller, or action method level with the `[AllowAnonymous]` attribute.</span></span> <span data-ttu-id="d19a5-359">Configuración de la directiva de autenticación predeterminado para exigir que los usuarios se autentiquen protege recién agregada de las páginas de Razor y controladores.</span><span class="sxs-lookup"><span data-stu-id="d19a5-359">Setting the default authentication policy to require users to be authenticated protects newly added Razor Pages and controllers.</span></span> <span data-ttu-id="d19a5-360">Tener autenticación necesaria de forma predeterminada es más seguro que confiar en los nuevos controladores y Razor Pages para incluir el atributo `[Authorize]`.</span><span class="sxs-lookup"><span data-stu-id="d19a5-360">Having authentication required by default is more secure than relying on new controllers and Razor Pages to include the `[Authorize]` attribute.</span></span>

<span data-ttu-id="d19a5-361">Agregue [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) a las páginas index, about y Contact para que los usuarios anónimos puedan obtener información sobre el sitio antes de registrarse.</span><span class="sxs-lookup"><span data-stu-id="d19a5-361">Add [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) to the Index, About, and Contact pages so anonymous users can get information about the site before they register.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Index.cshtml.cs?highlight=1,6)]

### <a name="configure-the-test-account"></a><span data-ttu-id="d19a5-362">Configurar la cuenta de prueba</span><span class="sxs-lookup"><span data-stu-id="d19a5-362">Configure the test account</span></span>

<span data-ttu-id="d19a5-363">La clase `SeedData` crea dos cuentas: administrador y administrador.</span><span class="sxs-lookup"><span data-stu-id="d19a5-363">The `SeedData` class creates two accounts: administrator and manager.</span></span> <span data-ttu-id="d19a5-364">Use la [herramienta Administrador de secretos](xref:security/app-secrets) para establecer una contraseña para estas cuentas.</span><span class="sxs-lookup"><span data-stu-id="d19a5-364">Use the [Secret Manager tool](xref:security/app-secrets) to set a password for these accounts.</span></span> <span data-ttu-id="d19a5-365">Establezca la contraseña desde el directorio del proyecto (el directorio que contiene *Program.CS*):</span><span class="sxs-lookup"><span data-stu-id="d19a5-365">Set the password from the project directory (the directory containing *Program.cs*):</span></span>

```dotnetcli
dotnet user-secrets set SeedUserPW <PW>
```

<span data-ttu-id="d19a5-366">Si no se especifica una contraseña segura, se produce una excepción cuando se llama a `SeedData.Initialize`.</span><span class="sxs-lookup"><span data-stu-id="d19a5-366">If a strong password is not specified, an exception is thrown when `SeedData.Initialize` is called.</span></span>

<span data-ttu-id="d19a5-367">Actualice `Main` para usar la contraseña de prueba:</span><span class="sxs-lookup"><span data-stu-id="d19a5-367">Update `Main` to use the test password:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a><span data-ttu-id="d19a5-368">Crear las cuentas de prueba y actualizar los contactos</span><span class="sxs-lookup"><span data-stu-id="d19a5-368">Create the test accounts and update the contacts</span></span>

<span data-ttu-id="d19a5-369">Actualice el método `Initialize` de la clase `SeedData` para crear las cuentas de prueba:</span><span class="sxs-lookup"><span data-stu-id="d19a5-369">Update the `Initialize` method in the `SeedData` class to create the test accounts:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Data/SeedData.cs?name=snippet_Initialize)]

<span data-ttu-id="d19a5-370">Agregue el identificador de usuario de administrador y el `ContactStatus` a los contactos.</span><span class="sxs-lookup"><span data-stu-id="d19a5-370">Add the administrator user ID and `ContactStatus` to the contacts.</span></span> <span data-ttu-id="d19a5-371">Realice uno de los contactos "Enviado" y un "rechazada".</span><span class="sxs-lookup"><span data-stu-id="d19a5-371">Make one of the contacts "Submitted" and one "Rejected".</span></span> <span data-ttu-id="d19a5-372">Agregue el Id. de usuario y el estado a todos los contactos.</span><span class="sxs-lookup"><span data-stu-id="d19a5-372">Add the user ID and status to all the contacts.</span></span> <span data-ttu-id="d19a5-373">Póngase en contacto un solo con se muestra:</span><span class="sxs-lookup"><span data-stu-id="d19a5-373">Only one contact is shown:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a><span data-ttu-id="d19a5-374">Crear controladores de autorización de administrador, administrador y propietario</span><span class="sxs-lookup"><span data-stu-id="d19a5-374">Create owner, manager, and administrator authorization handlers</span></span>

<span data-ttu-id="d19a5-375">Cree una carpeta de *autorización* y cree una `ContactIsOwnerAuthorizationHandler` clase en ella.</span><span class="sxs-lookup"><span data-stu-id="d19a5-375">Create an *Authorization* folder and create a `ContactIsOwnerAuthorizationHandler` class in it.</span></span> <span data-ttu-id="d19a5-376">En el `ContactIsOwnerAuthorizationHandler` se comprueba que el usuario que actúa en un recurso posee el recurso.</span><span class="sxs-lookup"><span data-stu-id="d19a5-376">The `ContactIsOwnerAuthorizationHandler` verifies that the user acting on a resource owns the resource.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

<span data-ttu-id="d19a5-377">El `ContactIsOwnerAuthorizationHandler` llama al [contexto. Se realiza correctamente](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) si el usuario autenticado actual es el propietario del contacto.</span><span class="sxs-lookup"><span data-stu-id="d19a5-377">The `ContactIsOwnerAuthorizationHandler` calls [context.Succeed](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) if the current authenticated user is the contact owner.</span></span> <span data-ttu-id="d19a5-378">Controladores de autorización con carácter general:</span><span class="sxs-lookup"><span data-stu-id="d19a5-378">Authorization handlers generally:</span></span>

* <span data-ttu-id="d19a5-379">Devuelve `context.Succeed` cuando se cumplen los requisitos.</span><span class="sxs-lookup"><span data-stu-id="d19a5-379">Return `context.Succeed` when the requirements are met.</span></span>
* <span data-ttu-id="d19a5-380">Devuelve `Task.CompletedTask` cuando no se cumplen los requisitos.</span><span class="sxs-lookup"><span data-stu-id="d19a5-380">Return `Task.CompletedTask` when requirements aren't met.</span></span> <span data-ttu-id="d19a5-381">`Task.CompletedTask` no es correcto o error&mdash;permite que se ejecuten otros controladores de autorización.</span><span class="sxs-lookup"><span data-stu-id="d19a5-381">`Task.CompletedTask` is not success or failure&mdash;it allows other authorization handlers to run.</span></span>

<span data-ttu-id="d19a5-382">Si necesita generar un error explícitamente, devuelva el [contexto. Error](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span><span class="sxs-lookup"><span data-stu-id="d19a5-382">If you need to explicitly fail, return [context.Fail](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span></span>

<span data-ttu-id="d19a5-383">La aplicación permite a los propietarios de contacto para editar, eliminar o crear sus propios datos.</span><span class="sxs-lookup"><span data-stu-id="d19a5-383">The app allows contact owners to edit/delete/create their own data.</span></span> <span data-ttu-id="d19a5-384">`ContactIsOwnerAuthorizationHandler` no necesita comprobar la operación que se ha pasado en el parámetro de requisito.</span><span class="sxs-lookup"><span data-stu-id="d19a5-384">`ContactIsOwnerAuthorizationHandler` doesn't need to check the operation passed in the requirement parameter.</span></span>

### <a name="create-a-manager-authorization-handler"></a><span data-ttu-id="d19a5-385">Crear un controlador de autorización de administrador</span><span class="sxs-lookup"><span data-stu-id="d19a5-385">Create a manager authorization handler</span></span>

<span data-ttu-id="d19a5-386">Cree una clase de `ContactManagerAuthorizationHandler` en la carpeta de *autorización* .</span><span class="sxs-lookup"><span data-stu-id="d19a5-386">Create a `ContactManagerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="d19a5-387">El `ContactManagerAuthorizationHandler` comprueba que el usuario que actúa en el recurso es un administrador.</span><span class="sxs-lookup"><span data-stu-id="d19a5-387">The `ContactManagerAuthorizationHandler` verifies the user acting on the resource is a manager.</span></span> <span data-ttu-id="d19a5-388">Solo los administradores pueden aprobar o rechazar los cambios de contenido (nuevos o modificados).</span><span class="sxs-lookup"><span data-stu-id="d19a5-388">Only managers can approve or reject content changes (new or changed).</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a><span data-ttu-id="d19a5-389">Cree un controlador de autorización de administrador</span><span class="sxs-lookup"><span data-stu-id="d19a5-389">Create an administrator authorization handler</span></span>

<span data-ttu-id="d19a5-390">Cree una clase de `ContactAdministratorsAuthorizationHandler` en la carpeta de *autorización* .</span><span class="sxs-lookup"><span data-stu-id="d19a5-390">Create a `ContactAdministratorsAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="d19a5-391">El `ContactAdministratorsAuthorizationHandler` comprueba que el usuario que actúa en el recurso es un administrador.</span><span class="sxs-lookup"><span data-stu-id="d19a5-391">The `ContactAdministratorsAuthorizationHandler` verifies the user acting on the resource is an administrator.</span></span> <span data-ttu-id="d19a5-392">Administrador puede realizar todas las operaciones.</span><span class="sxs-lookup"><span data-stu-id="d19a5-392">Administrator can do all operations.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a><span data-ttu-id="d19a5-393">Registrar los controladores de autorización</span><span class="sxs-lookup"><span data-stu-id="d19a5-393">Register the authorization handlers</span></span>

<span data-ttu-id="d19a5-394">Los servicios que usan Entity Framework Core deben estar registrados para la [inserción de dependencias](xref:fundamentals/dependency-injection) mediante [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span><span class="sxs-lookup"><span data-stu-id="d19a5-394">Services using Entity Framework Core must be registered for [dependency injection](xref:fundamentals/dependency-injection) using [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span></span> <span data-ttu-id="d19a5-395">En el `ContactIsOwnerAuthorizationHandler` se usa ASP.NET Core [Identity](xref:security/authentication/identity), que se basa en Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="d19a5-395">The `ContactIsOwnerAuthorizationHandler` uses ASP.NET Core [Identity](xref:security/authentication/identity), which is built on Entity Framework Core.</span></span> <span data-ttu-id="d19a5-396">Registre los controladores con la colección de servicios para que estén disponibles para el `ContactsController` a través de la [inserción de dependencias](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="d19a5-396">Register the handlers with the service collection so they're available to the `ContactsController` through [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="d19a5-397">Agregue el código siguiente al final de `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="d19a5-397">Add the following code to the end of `ConfigureServices`:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet_defaultPolicy&highlight=27-99)]

<span data-ttu-id="d19a5-398">`ContactAdministratorsAuthorizationHandler` y `ContactManagerAuthorizationHandler` se agregan como singletons.</span><span class="sxs-lookup"><span data-stu-id="d19a5-398">`ContactAdministratorsAuthorizationHandler` and `ContactManagerAuthorizationHandler` are added as singletons.</span></span> <span data-ttu-id="d19a5-399">Son singleton porque no usan EF y toda la información necesaria está en el parámetro `Context` del método `HandleRequirementAsync`.</span><span class="sxs-lookup"><span data-stu-id="d19a5-399">They're singletons because they don't use EF and all the information needed is in the `Context` parameter of the `HandleRequirementAsync` method.</span></span>

## <a name="support-authorization"></a><span data-ttu-id="d19a5-400">Admitir la autorización</span><span class="sxs-lookup"><span data-stu-id="d19a5-400">Support authorization</span></span>

<span data-ttu-id="d19a5-401">En esta sección, actualice las páginas de Razor y agregue una clase de los requisitos de operaciones.</span><span class="sxs-lookup"><span data-stu-id="d19a5-401">In this section, you update the Razor Pages and add an operations requirements class.</span></span>

### <a name="review-the-contact-operations-requirements-class"></a><span data-ttu-id="d19a5-402">Revisión de la clase de los requisitos de las operaciones de contacto</span><span class="sxs-lookup"><span data-stu-id="d19a5-402">Review the contact operations requirements class</span></span>

<span data-ttu-id="d19a5-403">Revise la clase `ContactOperations`.</span><span class="sxs-lookup"><span data-stu-id="d19a5-403">Review the `ContactOperations` class.</span></span> <span data-ttu-id="d19a5-404">Esta clase contiene los requisitos que admite la aplicación:</span><span class="sxs-lookup"><span data-stu-id="d19a5-404">This class contains the requirements the app supports:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-contacts-razor-pages"></a><span data-ttu-id="d19a5-405">Crear una clase base para las páginas de Razor de contactos</span><span class="sxs-lookup"><span data-stu-id="d19a5-405">Create a base class for the Contacts Razor Pages</span></span>

<span data-ttu-id="d19a5-406">Cree una clase base que contiene los servicios usados en los contactos de las páginas de Razor.</span><span class="sxs-lookup"><span data-stu-id="d19a5-406">Create a base class that contains the services used in the contacts Razor Pages.</span></span> <span data-ttu-id="d19a5-407">La clase base coloca el código de inicialización en una ubicación:</span><span class="sxs-lookup"><span data-stu-id="d19a5-407">The base class puts the initialization code in one location:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/DI_BasePageModel.cs)]

<span data-ttu-id="d19a5-408">El código anterior:</span><span class="sxs-lookup"><span data-stu-id="d19a5-408">The preceding code:</span></span>

* <span data-ttu-id="d19a5-409">Agrega el servicio `IAuthorizationService` para tener acceso a los controladores de autorización.</span><span class="sxs-lookup"><span data-stu-id="d19a5-409">Adds the `IAuthorizationService` service to access to the authorization handlers.</span></span>
* <span data-ttu-id="d19a5-410">Agrega el servicio Identity `UserManager`.</span><span class="sxs-lookup"><span data-stu-id="d19a5-410">Adds the Identity `UserManager` service.</span></span>
* <span data-ttu-id="d19a5-411">Agregue la `ApplicationDbContext`.</span><span class="sxs-lookup"><span data-stu-id="d19a5-411">Add the `ApplicationDbContext`.</span></span>

### <a name="update-the-createmodel"></a><span data-ttu-id="d19a5-412">Actualizar el CreateModel</span><span class="sxs-lookup"><span data-stu-id="d19a5-412">Update the CreateModel</span></span>

<span data-ttu-id="d19a5-413">Actualice el constructor Create Page Model para usar la clase base `DI_BasePageModel`:</span><span class="sxs-lookup"><span data-stu-id="d19a5-413">Update the create page model constructor to use the `DI_BasePageModel` base class:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

<span data-ttu-id="d19a5-414">Actualice el método `CreateModel.OnPostAsync` para:</span><span class="sxs-lookup"><span data-stu-id="d19a5-414">Update the `CreateModel.OnPostAsync` method to:</span></span>

* <span data-ttu-id="d19a5-415">Agregue el identificador de usuario al modelo de `Contact`.</span><span class="sxs-lookup"><span data-stu-id="d19a5-415">Add the user ID to the `Contact` model.</span></span>
* <span data-ttu-id="d19a5-416">Llamar al controlador de autorización para comprobar que el usuario tiene permiso para crear contactos.</span><span class="sxs-lookup"><span data-stu-id="d19a5-416">Call the authorization handler to verify the user has permission to create contacts.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a><span data-ttu-id="d19a5-417">Actualizar el IndexModel</span><span class="sxs-lookup"><span data-stu-id="d19a5-417">Update the IndexModel</span></span>

<span data-ttu-id="d19a5-418">Actualice el método de `OnGetAsync` de modo que solo se muestren los contactos aprobados a los usuarios generales:</span><span class="sxs-lookup"><span data-stu-id="d19a5-418">Update the `OnGetAsync` method so only approved contacts are shown to general users:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a><span data-ttu-id="d19a5-419">Actualizar el EditModel</span><span class="sxs-lookup"><span data-stu-id="d19a5-419">Update the EditModel</span></span>

<span data-ttu-id="d19a5-420">Agregar un controlador de autorización para comprobar que el usuario propietario del contacto.</span><span class="sxs-lookup"><span data-stu-id="d19a5-420">Add an authorization handler to verify the user owns the contact.</span></span> <span data-ttu-id="d19a5-421">Dado que se está validando la autorización de recursos, el atributo `[Authorize]` no es suficiente.</span><span class="sxs-lookup"><span data-stu-id="d19a5-421">Because resource authorization is being validated, the `[Authorize]` attribute is not enough.</span></span> <span data-ttu-id="d19a5-422">La aplicación no tiene acceso al recurso cuando se evalúan los atributos.</span><span class="sxs-lookup"><span data-stu-id="d19a5-422">The app doesn't have access to the resource when attributes are evaluated.</span></span> <span data-ttu-id="d19a5-423">Autorización basada en el recurso debe ser un imperativo.</span><span class="sxs-lookup"><span data-stu-id="d19a5-423">Resource-based authorization must be imperative.</span></span> <span data-ttu-id="d19a5-424">Las comprobaciones deben realizarse una vez que la aplicación tenga acceso al recurso, cargándolo en el modelo de página o cargándola en el controlador de sí mismo.</span><span class="sxs-lookup"><span data-stu-id="d19a5-424">Checks must be performed once the app has access to the resource, either by loading it in the page model or by loading it within the handler itself.</span></span> <span data-ttu-id="d19a5-425">Con frecuencia acceder al recurso pasando la clave de recurso.</span><span class="sxs-lookup"><span data-stu-id="d19a5-425">You frequently access the resource by passing in the resource key.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a><span data-ttu-id="d19a5-426">Actualizar el DeleteModel</span><span class="sxs-lookup"><span data-stu-id="d19a5-426">Update the DeleteModel</span></span>

<span data-ttu-id="d19a5-427">Actualice el modelo de página de delete para usar el controlador de autorización para comprobar que el usuario tiene permiso delete en el contacto.</span><span class="sxs-lookup"><span data-stu-id="d19a5-427">Update the delete page model to use the authorization handler to verify the user has delete permission on the contact.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a><span data-ttu-id="d19a5-428">Insertar el servicio de autorización en las vistas</span><span class="sxs-lookup"><span data-stu-id="d19a5-428">Inject the authorization service into the views</span></span>

<span data-ttu-id="d19a5-429">Actualmente, se muestra en la interfaz de usuario edita y elimina los vínculos para los contactos que no se puede modificar el usuario.</span><span class="sxs-lookup"><span data-stu-id="d19a5-429">Currently, the UI shows edit and delete links for contacts the user can't modify.</span></span>

<span data-ttu-id="d19a5-430">Inserte el servicio de autorización en el archivo *views/_ViewImports. cshtml* para que esté disponible para todas las vistas:</span><span class="sxs-lookup"><span data-stu-id="d19a5-430">Inject the authorization service in the *Views/_ViewImports.cshtml* file so it's available to all views:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/_ViewImports.cshtml?highlight=6-99)]

<span data-ttu-id="d19a5-431">El marcado anterior agrega varias instrucciones de `using`.</span><span class="sxs-lookup"><span data-stu-id="d19a5-431">The preceding markup adds several `using` statements.</span></span>

<span data-ttu-id="d19a5-432">Actualice los vínculos de **edición** y **eliminación** en *pages/Contacts/index. cshtml* para que solo se representen para los usuarios con los permisos adecuados:</span><span class="sxs-lookup"><span data-stu-id="d19a5-432">Update the **Edit** and **Delete** links in *Pages/Contacts/Index.cshtml* so they're only rendered for users with the appropriate permissions:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Index.cshtml?highlight=34-36,62-999)]

> [!WARNING]
> <span data-ttu-id="d19a5-433">Ocultar los vínculos de los usuarios que no tienen permiso para cambiar los datos no proteger la aplicación.</span><span class="sxs-lookup"><span data-stu-id="d19a5-433">Hiding links from users that don't have permission to change data doesn't secure the app.</span></span> <span data-ttu-id="d19a5-434">Ocultar vínculos hace que la aplicación más fácil de usar mostrando vínculos solo es válido.</span><span class="sxs-lookup"><span data-stu-id="d19a5-434">Hiding links makes the app more user-friendly by displaying only valid links.</span></span> <span data-ttu-id="d19a5-435">Los usuarios pueden hack las direcciones URL generadas para invocar Editar y eliminar operaciones en los datos que no poseen.</span><span class="sxs-lookup"><span data-stu-id="d19a5-435">Users can hack the generated URLs to invoke edit and delete operations on data they don't own.</span></span> <span data-ttu-id="d19a5-436">La página de Razor o el controlador debe exigir comprobaciones de acceso para proteger los datos.</span><span class="sxs-lookup"><span data-stu-id="d19a5-436">The Razor Page or controller must enforce access checks to secure the data.</span></span>

### <a name="update-details"></a><span data-ttu-id="d19a5-437">Detalles de la actualización</span><span class="sxs-lookup"><span data-stu-id="d19a5-437">Update Details</span></span>

<span data-ttu-id="d19a5-438">Actualice la vista de detalles para que los administradores pueden aprobar o rechazar contactos:</span><span class="sxs-lookup"><span data-stu-id="d19a5-438">Update the details view so managers can approve or reject contacts:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml?name=snippet)]

<span data-ttu-id="d19a5-439">Actualice el modelo de página de detalles:</span><span class="sxs-lookup"><span data-stu-id="d19a5-439">Update the details page model:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="add-or-remove-a-user-to-a-role"></a><span data-ttu-id="d19a5-440">Agregar o quitar un usuario a un rol</span><span class="sxs-lookup"><span data-stu-id="d19a5-440">Add or remove a user to a role</span></span>

<span data-ttu-id="d19a5-441">Consulte [este problema](https://github.com/aspnet/AspNetCore.Docs/issues/8502) para obtener información sobre:</span><span class="sxs-lookup"><span data-stu-id="d19a5-441">See [this issue](https://github.com/aspnet/AspNetCore.Docs/issues/8502) for information on:</span></span>

* <span data-ttu-id="d19a5-442">Quita los privilegios de un usuario.</span><span class="sxs-lookup"><span data-stu-id="d19a5-442">Removing privileges from a user.</span></span> <span data-ttu-id="d19a5-443">Por ejemplo, silenciar a un usuario en una aplicación de chat.</span><span class="sxs-lookup"><span data-stu-id="d19a5-443">For example, muting a user in a chat app.</span></span>
* <span data-ttu-id="d19a5-444">Agregar privilegios a un usuario.</span><span class="sxs-lookup"><span data-stu-id="d19a5-444">Adding privileges to a user.</span></span>

## <a name="test-the-completed-app"></a><span data-ttu-id="d19a5-445">Probar la aplicación completada</span><span class="sxs-lookup"><span data-stu-id="d19a5-445">Test the completed app</span></span>

<span data-ttu-id="d19a5-446">Si aún no ha establecido una contraseña para las cuentas de usuario inicializadas, use la [herramienta Administrador de secretos](xref:security/app-secrets#secret-manager) para establecer una contraseña:</span><span class="sxs-lookup"><span data-stu-id="d19a5-446">If you haven't already set a password for seeded user accounts, use the [Secret Manager tool](xref:security/app-secrets#secret-manager) to set a password:</span></span>

* <span data-ttu-id="d19a5-447">Elija una contraseña segura: Use ocho o más caracteres y al menos una mayúscula, número y símbolos.</span><span class="sxs-lookup"><span data-stu-id="d19a5-447">Choose a strong password: Use eight or more characters and at least one upper-case character, number, and symbol.</span></span> <span data-ttu-id="d19a5-448">Por ejemplo, `Passw0rd!` cumple los requisitos de contraseñas seguras.</span><span class="sxs-lookup"><span data-stu-id="d19a5-448">For example, `Passw0rd!` meets the strong password requirements.</span></span>
* <span data-ttu-id="d19a5-449">Ejecute el siguiente comando desde la carpeta del proyecto, donde `<PW>` es la contraseña:</span><span class="sxs-lookup"><span data-stu-id="d19a5-449">Execute the following command from the project's folder, where `<PW>` is the password:</span></span>

  ```dotnetcli
  dotnet user-secrets set SeedUserPW <PW>
  ```

* <span data-ttu-id="d19a5-450">Quitar y actualizar la base de datos</span><span class="sxs-lookup"><span data-stu-id="d19a5-450">Drop and update the Database</span></span>

  ```dotnetcli
  dotnet ef database drop -f
  dotnet ef database update  
  ```

* <span data-ttu-id="d19a5-451">Reinicie la aplicación para inicializar la base de datos.</span><span class="sxs-lookup"><span data-stu-id="d19a5-451">Restart the app to seed the database.</span></span>

<span data-ttu-id="d19a5-452">Es una manera fácil de probar la aplicación completa iniciar los tres distintos exploradores (o las sesiones de InPrivate o incognito).</span><span class="sxs-lookup"><span data-stu-id="d19a5-452">An easy way to test the completed app is to launch three different browsers (or incognito/InPrivate sessions).</span></span> <span data-ttu-id="d19a5-453">En un explorador, registre un nuevo usuario (por ejemplo, `test@example.com`).</span><span class="sxs-lookup"><span data-stu-id="d19a5-453">In one browser, register a new user (for example, `test@example.com`).</span></span> <span data-ttu-id="d19a5-454">Inicie sesión con un usuario diferente en cada explorador.</span><span class="sxs-lookup"><span data-stu-id="d19a5-454">Sign in to each browser with a different user.</span></span> <span data-ttu-id="d19a5-455">Compruebe las siguientes operaciones:</span><span class="sxs-lookup"><span data-stu-id="d19a5-455">Verify the following operations:</span></span>

* <span data-ttu-id="d19a5-456">Usuarios registrados pueden ver todos los datos de contacto aprobados.</span><span class="sxs-lookup"><span data-stu-id="d19a5-456">Registered users can view all of the approved contact data.</span></span>
* <span data-ttu-id="d19a5-457">Los usuarios registrados pueden editar o eliminar sus propios datos.</span><span class="sxs-lookup"><span data-stu-id="d19a5-457">Registered users can edit/delete their own data.</span></span>
* <span data-ttu-id="d19a5-458">Los administradores pueden aprobar o rechazar datos de contacto.</span><span class="sxs-lookup"><span data-stu-id="d19a5-458">Managers can approve/reject contact data.</span></span> <span data-ttu-id="d19a5-459">En la vista `Details` se muestran los botones **aprobar** y **rechazar** .</span><span class="sxs-lookup"><span data-stu-id="d19a5-459">The `Details` view shows **Approve** and **Reject** buttons.</span></span>
* <span data-ttu-id="d19a5-460">Los administradores pueden aprobar o rechazar y editar o eliminar todos los datos.</span><span class="sxs-lookup"><span data-stu-id="d19a5-460">Administrators can approve/reject and edit/delete all data.</span></span>

| <span data-ttu-id="d19a5-461">Usuario</span><span class="sxs-lookup"><span data-stu-id="d19a5-461">User</span></span>                | <span data-ttu-id="d19a5-462">Propagadas por la aplicación</span><span class="sxs-lookup"><span data-stu-id="d19a5-462">Seeded by the app</span></span> | <span data-ttu-id="d19a5-463">Opciones</span><span class="sxs-lookup"><span data-stu-id="d19a5-463">Options</span></span>                                  |
| ------------------- | :---------------: | ---------------------------------------- |
| test@example.com    | <span data-ttu-id="d19a5-464">No</span><span class="sxs-lookup"><span data-stu-id="d19a5-464">No</span></span>                | <span data-ttu-id="d19a5-465">Editar o eliminar los datos propios.</span><span class="sxs-lookup"><span data-stu-id="d19a5-465">Edit/delete the own data.</span></span>                |
| manager@contoso.com | <span data-ttu-id="d19a5-466">Sí</span><span class="sxs-lookup"><span data-stu-id="d19a5-466">Yes</span></span>               | <span data-ttu-id="d19a5-467">Aprobar o rechazar y editar o eliminar los datos propios.</span><span class="sxs-lookup"><span data-stu-id="d19a5-467">Approve/reject and edit/delete own data.</span></span> |
| admin@contoso.com   | <span data-ttu-id="d19a5-468">Sí</span><span class="sxs-lookup"><span data-stu-id="d19a5-468">Yes</span></span>               | <span data-ttu-id="d19a5-469">Aprobar o rechazar y editar o eliminar todos los datos.</span><span class="sxs-lookup"><span data-stu-id="d19a5-469">Approve/reject and edit/delete all data.</span></span> |

<span data-ttu-id="d19a5-470">Crear un contacto en el explorador del administrador.</span><span class="sxs-lookup"><span data-stu-id="d19a5-470">Create a contact in the administrator's browser.</span></span> <span data-ttu-id="d19a5-471">Copie la dirección URL para su eliminación y editar en el contacto del administrador.</span><span class="sxs-lookup"><span data-stu-id="d19a5-471">Copy the URL for delete and edit from the administrator contact.</span></span> <span data-ttu-id="d19a5-472">Pegue estos vínculos en el explorador del usuario de prueba para comprobar que el usuario de prueba no puede realizar estas operaciones.</span><span class="sxs-lookup"><span data-stu-id="d19a5-472">Paste these links into the test user's browser to verify the test user can't perform these operations.</span></span>

## <a name="create-the-starter-app"></a><span data-ttu-id="d19a5-473">Crear la aplicación de inicio</span><span class="sxs-lookup"><span data-stu-id="d19a5-473">Create the starter app</span></span>

* <span data-ttu-id="d19a5-474">Cree una aplicación de páginas de Razor denominada "ContactManager"</span><span class="sxs-lookup"><span data-stu-id="d19a5-474">Create a Razor Pages app named "ContactManager"</span></span>
  * <span data-ttu-id="d19a5-475">Cree la aplicación con **cuentas de usuario individuales**.</span><span class="sxs-lookup"><span data-stu-id="d19a5-475">Create the app with **Individual User Accounts**.</span></span>
  * <span data-ttu-id="d19a5-476">Asígnele el nombre "ContactManager" para el espacio de nombres coincida con el espacio de nombres utilizado en el ejemplo.</span><span class="sxs-lookup"><span data-stu-id="d19a5-476">Name it "ContactManager" so the namespace matches the namespace used in the sample.</span></span>
  * <span data-ttu-id="d19a5-477">`-uld` especifica LocalDB en lugar de SQLite</span><span class="sxs-lookup"><span data-stu-id="d19a5-477">`-uld` specifies LocalDB instead of SQLite</span></span>

  ```dotnetcli
  dotnet new webapp -o ContactManager -au Individual -uld
  ```

* <span data-ttu-id="d19a5-478">Agregue *Models/contact. CS*:</span><span class="sxs-lookup"><span data-stu-id="d19a5-478">Add *Models/Contact.cs*:</span></span>

  [!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

* <span data-ttu-id="d19a5-479">Aplicar scaffolding al modelo de `Contact`.</span><span class="sxs-lookup"><span data-stu-id="d19a5-479">Scaffold the `Contact` model.</span></span>
* <span data-ttu-id="d19a5-480">Crear la migración inicial y actualizar la base de datos:</span><span class="sxs-lookup"><span data-stu-id="d19a5-480">Create initial migration and update the database:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
  dotnet ef database drop -f
  dotnet ef migrations add initial
  dotnet ef database update
  ```

* <span data-ttu-id="d19a5-481">Actualice el delimitador **ContactManager** en el archivo *pages/_Layout. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="d19a5-481">Update the **ContactManager** anchor in the *Pages/_Layout.cshtml* file:</span></span>

  ```cshtml
  <a asp-page="/Contacts/Index" class="navbar-brand">ContactManager</a>
  ```

* <span data-ttu-id="d19a5-482">Probar la aplicación mediante la creación, edición y eliminación de un contacto</span><span class="sxs-lookup"><span data-stu-id="d19a5-482">Test the app by creating, editing, and deleting a contact</span></span>

### <a name="seed-the-database"></a><span data-ttu-id="d19a5-483">Inicializar la base de datos</span><span class="sxs-lookup"><span data-stu-id="d19a5-483">Seed the database</span></span>

<span data-ttu-id="d19a5-484">Agregue la clase [SeedData](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2.1/Data/SeedData.cs) a la carpeta de *datos* .</span><span class="sxs-lookup"><span data-stu-id="d19a5-484">Add the [SeedData](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2.1/Data/SeedData.cs) class to the *Data* folder.</span></span>

<span data-ttu-id="d19a5-485">Llame a `SeedData.Initialize` desde `Main`:</span><span class="sxs-lookup"><span data-stu-id="d19a5-485">Call `SeedData.Initialize` from `Main`:</span></span>

[!code-csharp[](secure-data/samples/starter2.1/Program.cs?name=snippet)]

<span data-ttu-id="d19a5-486">Probar que la aplicación había propagado la base de datos.</span><span class="sxs-lookup"><span data-stu-id="d19a5-486">Test that the app seeded the database.</span></span> <span data-ttu-id="d19a5-487">Si no hay ninguna fila en la base de datos de contacto, no se ejecuta el método de inicialización.</span><span class="sxs-lookup"><span data-stu-id="d19a5-487">If there are any rows in the contact DB, the seed method doesn't run.</span></span>

::: moniker-end

<a name="secure-data-add-resources-label"></a>

### <a name="additional-resources"></a><span data-ttu-id="d19a5-488">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="d19a5-488">Additional resources</span></span>

* [<span data-ttu-id="d19a5-489">Compilación de una aplicación Web de .NET Core y SQL Database en Azure App Service</span><span class="sxs-lookup"><span data-stu-id="d19a5-489">Build a .NET Core and SQL Database web app in Azure App Service</span></span>](/azure/app-service/app-service-web-tutorial-dotnetcore-sqldb)
* <span data-ttu-id="d19a5-490">[ASP.net Core laboratorio de autorización](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span><span class="sxs-lookup"><span data-stu-id="d19a5-490">[ASP.NET Core Authorization Lab](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span></span> <span data-ttu-id="d19a5-491">Este laboratorio explica con más detalle en las características de seguridad, presentadas en este tutorial.</span><span class="sxs-lookup"><span data-stu-id="d19a5-491">This lab goes into more detail on the security features introduced in this tutorial.</span></span>
* <xref:security/authorization/introduction>
* [<span data-ttu-id="d19a5-492">Autorización personalizada basada en directivas</span><span class="sxs-lookup"><span data-stu-id="d19a5-492">Custom policy-based authorization</span></span>](xref:security/authorization/policies)
