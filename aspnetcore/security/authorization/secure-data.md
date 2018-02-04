---
title: "Crear una aplicación de ASP.NET Core con datos de usuario protegidos por autorización"
author: rick-anderson
description: "Obtenga información acerca de cómo crear una aplicación de páginas de Razor con datos de usuario protegidos mediante autorización. Incluye SSL, la autenticación, la seguridad, ASP.NET Core Identity."
manager: wpickett
ms.author: riande
ms.date: 01/24/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/secure-data
ms.openlocfilehash: 6333082a2b2b4f6d3f1ce2afc600b4203a0f5dca
ms.sourcegitcommit: 7a87d66cf1d01febe6635c7306f2f679434901d1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/03/2018
---
# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a><span data-ttu-id="e5600-104">Crear una aplicación de ASP.NET Core con datos de usuario protegidos por autorización</span><span class="sxs-lookup"><span data-stu-id="e5600-104">Create an ASP.NET Core app with user data protected by authorization</span></span>

<span data-ttu-id="e5600-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT) y [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="e5600-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="e5600-106">Este tutorial muestra cómo crear una aplicación web de ASP.NET Core con datos de usuario protegidos mediante autorización.</span><span class="sxs-lookup"><span data-stu-id="e5600-106">This tutorial shows how to create an ASP.NET Core web app with user data protected by authorization.</span></span> <span data-ttu-id="e5600-107">Muestra una lista de contactos que autentica los usuarios (registrados) ha creado.</span><span class="sxs-lookup"><span data-stu-id="e5600-107">It displays a list of contacts that authenticated (registered) users have created.</span></span> <span data-ttu-id="e5600-108">Hay tres grupos de seguridad:</span><span class="sxs-lookup"><span data-stu-id="e5600-108">There are three security groups:</span></span>

* <span data-ttu-id="e5600-109">**Usuarios registrados** puede ver todos los datos aprobados y editar puede/eliminar sus propios datos.</span><span class="sxs-lookup"><span data-stu-id="e5600-109">**Registered users** can view all the approved data and can edit/delete their own data.</span></span>
* <span data-ttu-id="e5600-110">**Administradores de** puede aprobar o rechazar los datos de contacto.</span><span class="sxs-lookup"><span data-stu-id="e5600-110">**Managers** can approve or reject contact data.</span></span> <span data-ttu-id="e5600-111">Sólo los contactos aprobados son visibles para los usuarios.</span><span class="sxs-lookup"><span data-stu-id="e5600-111">Only approved contacts are visible to users.</span></span>
* <span data-ttu-id="e5600-112">**Los administradores** puede aprobar o rechazar y editar o eliminar los datos.</span><span class="sxs-lookup"><span data-stu-id="e5600-112">**Administrators** can approve/reject and edit/delete any data.</span></span>

<span data-ttu-id="e5600-113">En la siguiente imagen, usuario Rick (`rick@example.com`) ha iniciado sesión.</span><span class="sxs-lookup"><span data-stu-id="e5600-113">In the following image, user Rick (`rick@example.com`) is signed in.</span></span> <span data-ttu-id="e5600-114">Rick solo puede ver los contactos aprobados y **editar**/**eliminar**/**crear nuevo** vínculos de sus contactos.</span><span class="sxs-lookup"><span data-stu-id="e5600-114">Rick can only view approved contacts and **Edit**/**Delete**/**Create New** links for his contacts.</span></span> <span data-ttu-id="e5600-115">El último registro, creado por Rick, muestra **editar** y **eliminar** vínculos.</span><span class="sxs-lookup"><span data-stu-id="e5600-115">Only the last record, created by Rick, displays **Edit** and **Delete** links.</span></span> <span data-ttu-id="e5600-116">Otros usuarios no verán el último registro hasta que un administrador o un administrador cambia el estado a "Aprobado".</span><span class="sxs-lookup"><span data-stu-id="e5600-116">Other users won't see the last record until a manager or administrator changes the status to "Approved".</span></span>

![imagen describe anterior](secure-data/_static/rick.png)

<span data-ttu-id="e5600-118">En la siguiente imagen, `manager@contoso.com` está firmado en y en el rol de administrador:</span><span class="sxs-lookup"><span data-stu-id="e5600-118">In the following image, `manager@contoso.com` is signed in and in the managers role:</span></span>

![imagen describe anterior](secure-data/_static/manager1.png)

<span data-ttu-id="e5600-120">La siguiente imagen muestra a los administradores de la vista de detalles de un contacto:</span><span class="sxs-lookup"><span data-stu-id="e5600-120">The following image shows the managers details view of a contact:</span></span>

![imagen describe anterior](secure-data/_static/manager.png)

<span data-ttu-id="e5600-122">El **aprobar** y **rechazar** solo se muestran los botones para los administradores y administradores.</span><span class="sxs-lookup"><span data-stu-id="e5600-122">The **Approve** and **Reject** buttons are only displayed for managers and administrators.</span></span>

<span data-ttu-id="e5600-123">En la siguiente imagen, `admin@contoso.com` está firmado en y en la función Administradores:</span><span class="sxs-lookup"><span data-stu-id="e5600-123">In the following image, `admin@contoso.com` is signed in and in the administrators role:</span></span>

![imagen describe anterior](secure-data/_static/admin.png)

<span data-ttu-id="e5600-125">El administrador tiene todos los privilegios.</span><span class="sxs-lookup"><span data-stu-id="e5600-125">The administrator has all privileges.</span></span> <span data-ttu-id="e5600-126">Puede leer, editar o eliminar cualquier contacto y cambiar el estado de contactos.</span><span class="sxs-lookup"><span data-stu-id="e5600-126">She can read/edit/delete any contact and change the status of contacts.</span></span>

<span data-ttu-id="e5600-127">La aplicación se creó con [scaffolding](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller) siguiente `Contact` modelo:</span><span class="sxs-lookup"><span data-stu-id="e5600-127">The app was created by [scaffolding](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller) the following `Contact` model:</span></span>

[!code-csharp[Main](secure-data/samples/starter2/Models/Contact.cs?name=snippet1)]

<span data-ttu-id="e5600-128">El ejemplo contiene los siguientes controladores de autorización:</span><span class="sxs-lookup"><span data-stu-id="e5600-128">The sample contains the following authorization handlers:</span></span>

* <span data-ttu-id="e5600-129">`ContactIsOwnerAuthorizationHandler`: Se asegura de que un usuario solo puede editar sus datos.</span><span class="sxs-lookup"><span data-stu-id="e5600-129">`ContactIsOwnerAuthorizationHandler`: Ensures that a user can only edit their data.</span></span>
* <span data-ttu-id="e5600-130">`ContactManagerAuthorizationHandler`: Permite a los administradores aprobar o rechazar los contactos.</span><span class="sxs-lookup"><span data-stu-id="e5600-130">`ContactManagerAuthorizationHandler`: Allows managers to approve or reject contacts.</span></span>
* <span data-ttu-id="e5600-131">`ContactAdministratorsAuthorizationHandler`: Permite a los administradores para aprobar o rechazar los contactos y editar o eliminar contactos.</span><span class="sxs-lookup"><span data-stu-id="e5600-131">`ContactAdministratorsAuthorizationHandler`: Allows administrators to approve or reject contacts and to edit/delete contacts.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e5600-132">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="e5600-132">Prerequisites</span></span>

<span data-ttu-id="e5600-133">Este tutorial se avanza.</span><span class="sxs-lookup"><span data-stu-id="e5600-133">This tutorial is advanced.</span></span> <span data-ttu-id="e5600-134">Debe estar familiarizado con:</span><span class="sxs-lookup"><span data-stu-id="e5600-134">You should be familiar with:</span></span>

* [<span data-ttu-id="e5600-135">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e5600-135">ASP.NET Core</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="e5600-136">Autenticación</span><span class="sxs-lookup"><span data-stu-id="e5600-136">Authentication</span></span>](xref:security/authentication/index)
* [<span data-ttu-id="e5600-137">Confirmación de cuentas y recuperación de contraseñas</span><span class="sxs-lookup"><span data-stu-id="e5600-137">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="e5600-138">Autorización</span><span class="sxs-lookup"><span data-stu-id="e5600-138">Authorization</span></span>](xref:security/authorization/index)
* [<span data-ttu-id="e5600-139">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="e5600-139">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

<span data-ttu-id="e5600-140">Vea [este archivo PDF](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) para la versión principal de ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="e5600-140">See [this PDF file](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core MVC version.</span></span> <span data-ttu-id="e5600-141">La versión 1.1 de ASP.NET Core de este tutorial está en [esto](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data) carpeta.</span><span class="sxs-lookup"><span data-stu-id="e5600-141">The ASP.NET Core 1.1 version of this tutorial is in [this](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data) folder.</span></span> <span data-ttu-id="e5600-142">La 1.1 incluye el ejemplo de ASP.NET Core el [ejemplos](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).</span><span class="sxs-lookup"><span data-stu-id="e5600-142">The 1.1 ASP.NET Core sample is in the [samples](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).</span></span>

## <a name="the-starter-and-completed-app"></a><span data-ttu-id="e5600-143">El inicio y la aplicación completada</span><span class="sxs-lookup"><span data-stu-id="e5600-143">The starter and completed app</span></span>

<span data-ttu-id="e5600-144">[Descargar](xref:tutorials/index#how-to-download-a-sample) el [completado](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2) aplicación.</span><span class="sxs-lookup"><span data-stu-id="e5600-144">[Download](xref:tutorials/index#how-to-download-a-sample) the [completed](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2) app.</span></span> <span data-ttu-id="e5600-145">[Prueba](#test-the-completed-app) la aplicación completada por lo que se familiarice con sus características de seguridad.</span><span class="sxs-lookup"><span data-stu-id="e5600-145">[Test](#test-the-completed-app) the completed app so you become familiar with its security features.</span></span>

### <a name="the-starter-app"></a><span data-ttu-id="e5600-146">La aplicación de inicio</span><span class="sxs-lookup"><span data-stu-id="e5600-146">The starter app</span></span>

<span data-ttu-id="e5600-147">[Descargar](xref:tutorials/index#how-to-download-a-sample) el [starter](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2) aplicación.</span><span class="sxs-lookup"><span data-stu-id="e5600-147">[Download](xref:tutorials/index#how-to-download-a-sample) the [starter](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2) app.</span></span>

<span data-ttu-id="e5600-148">Ejecutar la aplicación, pulse el **ContactManager** vincular y compruebe que puede crear, editar y eliminar un contacto.</span><span class="sxs-lookup"><span data-stu-id="e5600-148">Run the app, tap the **ContactManager** link, and verify you can create, edit, and delete a contact.</span></span>

## <a name="secure-user-data"></a><span data-ttu-id="e5600-149">Proteger los datos de usuario</span><span class="sxs-lookup"><span data-stu-id="e5600-149">Secure user data</span></span>

<span data-ttu-id="e5600-150">Las siguientes secciones contienen todos los pasos principales para crear la aplicación de datos de usuario seguras.</span><span class="sxs-lookup"><span data-stu-id="e5600-150">The following sections have all the major steps to create the secure user data app.</span></span> <span data-ttu-id="e5600-151">Le resultará útil para hacer referencia al proyecto completado.</span><span class="sxs-lookup"><span data-stu-id="e5600-151">You may find it helpful to refer to the completed project.</span></span>

### <a name="tie-the-contact-data-to-the-user"></a><span data-ttu-id="e5600-152">Asociar los datos de contacto para el usuario</span><span class="sxs-lookup"><span data-stu-id="e5600-152">Tie the contact data to the user</span></span>

<span data-ttu-id="e5600-153">Usar ASP.NET [identidad](xref:security/authentication/identity) Id. de usuario para garantizar que los usuarios puede editar sus datos, pero no otros datos de los usuarios.</span><span class="sxs-lookup"><span data-stu-id="e5600-153">Use the ASP.NET [Identity](xref:security/authentication/identity) user ID to ensure users can edit their data, but not other users data.</span></span> <span data-ttu-id="e5600-154">Agregar `OwnerID` y `ContactStatus` a la `Contact` modelo:</span><span class="sxs-lookup"><span data-stu-id="e5600-154">Add `OwnerID` and `ContactStatus` to the `Contact` model:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Models/Contact.cs?name=snippet1&highlight=5-6,16-999)]

<span data-ttu-id="e5600-155">`OwnerID`es el identificador del usuario desde el `AspNetUser` tabla el [identidad](xref:security/authentication/identity) base de datos.</span><span class="sxs-lookup"><span data-stu-id="e5600-155">`OwnerID` is the user's ID from the `AspNetUser` table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="e5600-156">El `Status` campo determina si un contacto es visible para los usuarios en general.</span><span class="sxs-lookup"><span data-stu-id="e5600-156">The `Status` field determines if a contact is viewable by general users.</span></span>

<span data-ttu-id="e5600-157">Crear una nueva migración y actualizar la base de datos:</span><span class="sxs-lookup"><span data-stu-id="e5600-157">Create a new migration and update the database:</span></span>

```console
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="require-ssl-and-authenticated-users"></a><span data-ttu-id="e5600-158">Requerir SSL y los usuarios autenticados</span><span class="sxs-lookup"><span data-stu-id="e5600-158">Require SSL and authenticated users</span></span>

<span data-ttu-id="e5600-159">Agregar [IHostingEnvironment](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) a `Startup`:</span><span class="sxs-lookup"><span data-stu-id="e5600-159">Add [IHostingEnvironment](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) to `Startup`:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Startup.cs?name=snippet_env)]

<span data-ttu-id="e5600-160">En el `ConfigureServices` método de la *Startup.cs* , agregue el [RequireHttpsAttribute](/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) filtro de autorización:</span><span class="sxs-lookup"><span data-stu-id="e5600-160">In the `ConfigureServices` method of the *Startup.cs* file, add the [RequireHttpsAttribute](/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) authorization filter:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Startup.cs?name=snippet_SSL&highlight=19-999)]

<span data-ttu-id="e5600-161">Si está utilizando Visual Studio, habilite SSL.</span><span class="sxs-lookup"><span data-stu-id="e5600-161">If you're using Visual Studio, enable SSL.</span></span>

<span data-ttu-id="e5600-162">Para redirigir las solicitudes HTTP a HTTPS, consulte [Middleware de reescritura de dirección URL](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="e5600-162">To redirect HTTP requests to HTTPS, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span> <span data-ttu-id="e5600-163">Si está usando código de Visual Studio o pruebas en una plataforma local que no incluye un certificado de prueba para SSL:</span><span class="sxs-lookup"><span data-stu-id="e5600-163">If you're using Visual Studio Code or testing on a local platform that doesn't include a test certificate for SSL:</span></span>

  <span data-ttu-id="e5600-164">Establecer `"LocalTest:skipSSL": true` en el *appsettings. Developement.JSON* archivo.</span><span class="sxs-lookup"><span data-stu-id="e5600-164">Set `"LocalTest:skipSSL": true` in the *appsettings.Developement.json* file.</span></span>

### <a name="require-authenticated-users"></a><span data-ttu-id="e5600-165">Requerir a los usuarios autenticados</span><span class="sxs-lookup"><span data-stu-id="e5600-165">Require authenticated users</span></span>

<span data-ttu-id="e5600-166">Establecer la directiva de autenticación predeterminado para exigir que los usuarios se autentiquen.</span><span class="sxs-lookup"><span data-stu-id="e5600-166">Set the default authentication policy to require users to be authenticated.</span></span> <span data-ttu-id="e5600-167">Puede rechazar la autenticación en el nivel de método página Razor, controlador o acción con el `[AllowAnonymous]` atributo.</span><span class="sxs-lookup"><span data-stu-id="e5600-167">You can opt out of authentication at the Razor Page, controller, or action method level with the `[AllowAnonymous]` attribute.</span></span> <span data-ttu-id="e5600-168">Establecer la directiva de autenticación predeterminado para exigir que los usuarios se autentiquen protege recién agregado las páginas Razor y controladores.</span><span class="sxs-lookup"><span data-stu-id="e5600-168">Setting the default authentication policy to require users to be authenticated protects newly added Razor Pages and controllers.</span></span> <span data-ttu-id="e5600-169">Existencia de autenticación que requiere de forma predeterminada es más segura que confiar en los nuevos controladores y las páginas de Razor para incluir la `[Authorize]` atributo.</span><span class="sxs-lookup"><span data-stu-id="e5600-169">Having authentication required by default is safer than relying on new controllers and Razor Pages to include the `[Authorize]` attribute.</span></span> <span data-ttu-id="e5600-170">Agregue lo siguiente a la `ConfigureServices` método de la *Startup.cs* archivo:</span><span class="sxs-lookup"><span data-stu-id="e5600-170">Add the following to the `ConfigureServices` method of the *Startup.cs* file:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Startup.cs?name=snippet_defaultPolicy&highlight=31-999)]

<span data-ttu-id="e5600-171">Agregar [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) al índice, las páginas sobre y, a continuación, póngase en contacto con por lo que los usuarios anónimos pueden recibir información sobre el sitio antes de que registrar.</span><span class="sxs-lookup"><span data-stu-id="e5600-171">Add [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) to the Index, About, and Contact pages so anonymous users can get information about the site before they register.</span></span> 

[!code-csharp[Main](secure-data/samples/final2/Pages/Index.cshtml.cs?name=snippet&highlight=2)]

<span data-ttu-id="e5600-172">Agregar `[AllowAnonymous]` a la [LoginModel y RegisterModel](https://github.com/aspnet/templating/issues/238).</span><span class="sxs-lookup"><span data-stu-id="e5600-172">Add `[AllowAnonymous]` to the [LoginModel and RegisterModel](https://github.com/aspnet/templating/issues/238).</span></span>

### <a name="configure-the-test-account"></a><span data-ttu-id="e5600-173">Configurar la cuenta de prueba</span><span class="sxs-lookup"><span data-stu-id="e5600-173">Configure the test account</span></span>

<span data-ttu-id="e5600-174">La `SeedData` clase crea dos cuentas: administrador y el administrador.</span><span class="sxs-lookup"><span data-stu-id="e5600-174">The `SeedData` class creates two accounts: administrator and manager.</span></span> <span data-ttu-id="e5600-175">Use la [herramienta secreto administrador](xref:security/app-secrets) para establecer una contraseña para estas cuentas.</span><span class="sxs-lookup"><span data-stu-id="e5600-175">Use the [Secret Manager tool](xref:security/app-secrets) to set a password for these accounts.</span></span> <span data-ttu-id="e5600-176">Establecer la contraseña desde el directorio del proyecto (el directorio que contiene *Program.cs*):</span><span class="sxs-lookup"><span data-stu-id="e5600-176">Set the password from the project directory (the directory containing *Program.cs*):</span></span>

```console
dotnet user-secrets set SeedUserPW <PW>
```

<span data-ttu-id="e5600-177">Actualización `Main` usar la contraseña de prueba:</span><span class="sxs-lookup"><span data-stu-id="e5600-177">Update `Main` to use the test password:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a><span data-ttu-id="e5600-178">Crear las cuentas de prueba y actualizar los contactos</span><span class="sxs-lookup"><span data-stu-id="e5600-178">Create the test accounts and update the contacts</span></span>

<span data-ttu-id="e5600-179">Actualización de la `Initialize` método en la `SeedData` clase para crear las cuentas de prueba:</span><span class="sxs-lookup"><span data-stu-id="e5600-179">Update the `Initialize` method in the `SeedData` class to create the test accounts:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Data/SeedData.cs?name=snippet_Initialize)]

<span data-ttu-id="e5600-180">Agregue el identificador de usuario de administrador y `ContactStatus` a los contactos.</span><span class="sxs-lookup"><span data-stu-id="e5600-180">Add the administrator user ID and `ContactStatus` to the contacts.</span></span> <span data-ttu-id="e5600-181">Realice uno de los contactos "Enviado" y un "rechazada".</span><span class="sxs-lookup"><span data-stu-id="e5600-181">Make one of the contacts "Submitted" and one "Rejected".</span></span> <span data-ttu-id="e5600-182">Agregue el Id. de usuario y el estado para todos los contactos.</span><span class="sxs-lookup"><span data-stu-id="e5600-182">Add the user ID and status to all the contacts.</span></span> <span data-ttu-id="e5600-183">Póngase en contacto un solo con se muestra:</span><span class="sxs-lookup"><span data-stu-id="e5600-183">Only one contact is shown:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a><span data-ttu-id="e5600-184">Crear propietario, el administrador y los controladores de autorización de administrador</span><span class="sxs-lookup"><span data-stu-id="e5600-184">Create owner, manager, and administrator authorization handlers</span></span>

<span data-ttu-id="e5600-185">Crear un `ContactIsOwnerAuthorizationHandler` clase en el *autorización* carpeta.</span><span class="sxs-lookup"><span data-stu-id="e5600-185">Create a `ContactIsOwnerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="e5600-186">El `ContactIsOwnerAuthorizationHandler` comprueba que el usuario que actúa en un recurso posee el recurso.</span><span class="sxs-lookup"><span data-stu-id="e5600-186">The `ContactIsOwnerAuthorizationHandler` verifies that the user acting on a resource owns the resource.</span></span>

[!code-csharp[Main](secure-data/samples/final2/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

<span data-ttu-id="e5600-187">El `ContactIsOwnerAuthorizationHandler` llamadas [contexto. Correctamente](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) si el usuario autenticado actual es el propietario del contacto.</span><span class="sxs-lookup"><span data-stu-id="e5600-187">The `ContactIsOwnerAuthorizationHandler` calls [context.Succeed](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) if the current authenticated user is the contact owner.</span></span> <span data-ttu-id="e5600-188">Controladores de autorización general:</span><span class="sxs-lookup"><span data-stu-id="e5600-188">Authorization handlers generally:</span></span>

* <span data-ttu-id="e5600-189">Devolver `context.Succeed` cuando se cumplen los requisitos.</span><span class="sxs-lookup"><span data-stu-id="e5600-189">Return `context.Succeed` when the requirements are met.</span></span>
* <span data-ttu-id="e5600-190">Devolver `Task.CompletedTask` cuando no se cumplen los requisitos.</span><span class="sxs-lookup"><span data-stu-id="e5600-190">Return `Task.CompletedTask` when requirements aren't met.</span></span> <span data-ttu-id="e5600-191">`Task.CompletedTask`no correcto o error&mdash;permite que otros controladores de autorización para que se ejecute.</span><span class="sxs-lookup"><span data-stu-id="e5600-191">`Task.CompletedTask` is neither success or failure&mdash;it allows other authorization handlers to run.</span></span>

<span data-ttu-id="e5600-192">Si necesita explícitamente un error, devolver [contexto. Un error](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span><span class="sxs-lookup"><span data-stu-id="e5600-192">If you need to explicitly fail, return [context.Fail](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span></span>

<span data-ttu-id="e5600-193">La aplicación permite a los propietarios de contacto para editar, eliminar o crear sus propios datos.</span><span class="sxs-lookup"><span data-stu-id="e5600-193">The app allows contact owners to edit/delete/create their own data.</span></span> <span data-ttu-id="e5600-194">`ContactIsOwnerAuthorizationHandler`no tiene que comprobar la operación pasada en el parámetro de requisito.</span><span class="sxs-lookup"><span data-stu-id="e5600-194">`ContactIsOwnerAuthorizationHandler` doesn't need to check the operation passed in the requirement parameter.</span></span>

### <a name="create-a-manager-authorization-handler"></a><span data-ttu-id="e5600-195">Crear un controlador del Administrador de autorización</span><span class="sxs-lookup"><span data-stu-id="e5600-195">Create a manager authorization handler</span></span>

<span data-ttu-id="e5600-196">Crear un `ContactManagerAuthorizationHandler` clase en el *autorización* carpeta.</span><span class="sxs-lookup"><span data-stu-id="e5600-196">Create a `ContactManagerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="e5600-197">El `ContactManagerAuthorizationHandler` comprueba que el usuario que actúa en el recurso es un administrador.</span><span class="sxs-lookup"><span data-stu-id="e5600-197">The `ContactManagerAuthorizationHandler` verifies the user acting on the resource is a manager.</span></span> <span data-ttu-id="e5600-198">Solo los administradores pueden aprobar o rechazar los cambios de contenido (nuevos o modificados).</span><span class="sxs-lookup"><span data-stu-id="e5600-198">Only managers can approve or reject content changes (new or changed).</span></span>

[!code-csharp[Main](secure-data/samples/final2/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a><span data-ttu-id="e5600-199">Crear un controlador de autorización de administrador</span><span class="sxs-lookup"><span data-stu-id="e5600-199">Create an administrator authorization handler</span></span>

<span data-ttu-id="e5600-200">Crear un `ContactAdministratorsAuthorizationHandler` clase en el *autorización* carpeta.</span><span class="sxs-lookup"><span data-stu-id="e5600-200">Create a `ContactAdministratorsAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="e5600-201">El `ContactAdministratorsAuthorizationHandler` comprueba que el usuario que actúa en el recurso es un administrador.</span><span class="sxs-lookup"><span data-stu-id="e5600-201">The `ContactAdministratorsAuthorizationHandler` verifies the user acting on the resource is an administrator.</span></span> <span data-ttu-id="e5600-202">Administrador puede hacer que todas las operaciones.</span><span class="sxs-lookup"><span data-stu-id="e5600-202">Administrator can do all operations.</span></span>

[!code-csharp[Main](secure-data/samples/final2/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a><span data-ttu-id="e5600-203">Registrar los controladores de autorización</span><span class="sxs-lookup"><span data-stu-id="e5600-203">Register the authorization handlers</span></span>

<span data-ttu-id="e5600-204">Servicios mediante Entity Framework Core deben estar registrados para [inyección de dependencia](xref:fundamentals/dependency-injection) con [AddScoped](/aspnet/core/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span><span class="sxs-lookup"><span data-stu-id="e5600-204">Services using Entity Framework Core must be registered for [dependency injection](xref:fundamentals/dependency-injection) using [AddScoped](/aspnet/core/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span></span> <span data-ttu-id="e5600-205">El `ContactIsOwnerAuthorizationHandler` usa ASP.NET Core [identidad](xref:security/authentication/identity), que se basa en Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="e5600-205">The `ContactIsOwnerAuthorizationHandler` uses ASP.NET Core [Identity](xref:security/authentication/identity), which is built on Entity Framework Core.</span></span> <span data-ttu-id="e5600-206">Registre los controladores con la colección de servicio para que estén disponibles para la `ContactsController` a través de [inyección de dependencia](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="e5600-206">Register the handlers with the service collection so they're available to the `ContactsController` through [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="e5600-207">Agregue el código siguiente al final de `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="e5600-207">Add the following code to the end of `ConfigureServices`:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Startup.cs?name=ConfigureServices&highlight=41-999)]

<span data-ttu-id="e5600-208">`ContactAdministratorsAuthorizationHandler`y `ContactManagerAuthorizationHandler` se agregan como singleton.</span><span class="sxs-lookup"><span data-stu-id="e5600-208">`ContactAdministratorsAuthorizationHandler` and `ContactManagerAuthorizationHandler` are added as singletons.</span></span> <span data-ttu-id="e5600-209">Son singletons porque no usan EF y toda la información necesaria se encuentra en la `Context` parámetro de la `HandleRequirementAsync` método.</span><span class="sxs-lookup"><span data-stu-id="e5600-209">They're singletons because they don't use EF and all the information needed is in the `Context` parameter of the `HandleRequirementAsync` method.</span></span>

## <a name="support-authorization"></a><span data-ttu-id="e5600-210">Admitir la autorización</span><span class="sxs-lookup"><span data-stu-id="e5600-210">Support authorization</span></span>

<span data-ttu-id="e5600-211">En esta sección, actualice las páginas Razor y agregar una clase de requisitos de las operaciones.</span><span class="sxs-lookup"><span data-stu-id="e5600-211">In this section, you update the Razor Pages and add an operations requirements class.</span></span>

### <a name="review-the-contact-operations-requirements-class"></a><span data-ttu-id="e5600-212">Revisión de la clase de requisitos de operaciones de contacto</span><span class="sxs-lookup"><span data-stu-id="e5600-212">Review the contact operations requirements class</span></span>

<span data-ttu-id="e5600-213">Revise la `ContactOperations` clase.</span><span class="sxs-lookup"><span data-stu-id="e5600-213">Review the `ContactOperations` class.</span></span> <span data-ttu-id="e5600-214">Esta clase contiene los requisitos de la aplicación admite:</span><span class="sxs-lookup"><span data-stu-id="e5600-214">This class contains the requirements the app supports:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-razor-pages"></a><span data-ttu-id="e5600-215">Crear una clase base para las páginas de Razor</span><span class="sxs-lookup"><span data-stu-id="e5600-215">Create a base class for the Razor Pages</span></span>

<span data-ttu-id="e5600-216">Cree una clase base que contiene los servicios usados en los contactos de las páginas de Razor.</span><span class="sxs-lookup"><span data-stu-id="e5600-216">Create a base class that contains the services used in the contacts Razor Pages.</span></span> <span data-ttu-id="e5600-217">La clase base coloca ese código de inicialización en una ubicación:</span><span class="sxs-lookup"><span data-stu-id="e5600-217">The base class puts that initialization code in one location:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/DI_BasePageModel.cs)]

<span data-ttu-id="e5600-218">El código anterior:</span><span class="sxs-lookup"><span data-stu-id="e5600-218">The preceding code:</span></span>

* <span data-ttu-id="e5600-219">Agrega el `IAuthorizationService` servicio para tener acceso a los controladores de autorización.</span><span class="sxs-lookup"><span data-stu-id="e5600-219">Adds the `IAuthorizationService` service to access to the authorization handlers.</span></span>
* <span data-ttu-id="e5600-220">Agrega la identidad `UserManager` servicio.</span><span class="sxs-lookup"><span data-stu-id="e5600-220">Adds the Identity `UserManager` service.</span></span>
* <span data-ttu-id="e5600-221">Agregue la `ApplicationDbContext`.</span><span class="sxs-lookup"><span data-stu-id="e5600-221">Add the `ApplicationDbContext`.</span></span>

### <a name="update-the-createmodel"></a><span data-ttu-id="e5600-222">Actualizar el CreateModel</span><span class="sxs-lookup"><span data-stu-id="e5600-222">Update the CreateModel</span></span>

<span data-ttu-id="e5600-223">Actualizar el constructor de modelo de página de create para usar la `DI_BasePageModel` clase base:</span><span class="sxs-lookup"><span data-stu-id="e5600-223">Update the create page model constructor to use the `DI_BasePageModel` base class:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

<span data-ttu-id="e5600-224">Actualización de la `CreateModel.OnPostAsync` método:</span><span class="sxs-lookup"><span data-stu-id="e5600-224">Update the `CreateModel.OnPostAsync` method to:</span></span>

* <span data-ttu-id="e5600-225">Agregar el identificador de usuario para el `Contact` modelo.</span><span class="sxs-lookup"><span data-stu-id="e5600-225">Add the user ID to the `Contact` model.</span></span>
* <span data-ttu-id="e5600-226">Llamar al controlador de autorización para comprobar que el usuario tiene permiso para crear contactos.</span><span class="sxs-lookup"><span data-stu-id="e5600-226">Call the authorization handler to verify the user has permission to create contacts.</span></span>

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a><span data-ttu-id="e5600-227">Actualizar la IndexModel</span><span class="sxs-lookup"><span data-stu-id="e5600-227">Update the IndexModel</span></span>

<span data-ttu-id="e5600-228">Actualización de la `OnGetAsync` método por lo que sólo los contactos aprobados se muestran a los usuarios en general:</span><span class="sxs-lookup"><span data-stu-id="e5600-228">Update the `OnGetAsync` method so only approved contacts are shown to general users:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a><span data-ttu-id="e5600-229">Actualizar la EditModel</span><span class="sxs-lookup"><span data-stu-id="e5600-229">Update the EditModel</span></span>

<span data-ttu-id="e5600-230">Agregue un controlador de autorización para comprobar que el usuario es propietario del contacto.</span><span class="sxs-lookup"><span data-stu-id="e5600-230">Add an authorization handler to verify the user owns the contact.</span></span> <span data-ttu-id="e5600-231">Dado que se está validando la autorización de recursos, el `[Authorize]` atributo no es suficiente.</span><span class="sxs-lookup"><span data-stu-id="e5600-231">Because resource authorization is being validated, the `[Authorize]` attribute is not enough.</span></span> <span data-ttu-id="e5600-232">La aplicación no tiene acceso al recurso cuando se evalúan los atributos.</span><span class="sxs-lookup"><span data-stu-id="e5600-232">The app doesn't have access to the resource when attributes are evaluated.</span></span> <span data-ttu-id="e5600-233">Autorización basada en recursos debe ser imperativo.</span><span class="sxs-lookup"><span data-stu-id="e5600-233">Resource-based authorization must be imperative.</span></span> <span data-ttu-id="e5600-234">Las comprobaciones deben realizarse una vez que la aplicación tenga acceso al recurso, cargándolo en el modelo de páginas o cargándolo en su propio controlador.</span><span class="sxs-lookup"><span data-stu-id="e5600-234">Checks must be performed once the app has access to the resource, either by loading it in the page model or by loading it within the handler itself.</span></span> <span data-ttu-id="e5600-235">Con frecuencia tener acceso al recurso pasando la clave de recurso.</span><span class="sxs-lookup"><span data-stu-id="e5600-235">You frequently access the resource by passing in the resource key.</span></span>

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a><span data-ttu-id="e5600-236">Actualizar la DeleteModel</span><span class="sxs-lookup"><span data-stu-id="e5600-236">Update the DeleteModel</span></span>

<span data-ttu-id="e5600-237">Actualizar el modelo de páginas de delete para usar el controlador de autorización para comprobar que el usuario tiene permiso delete en el contacto.</span><span class="sxs-lookup"><span data-stu-id="e5600-237">Update the delete page model to use the authorization handler to verify the user has delete permission on the contact.</span></span>

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a><span data-ttu-id="e5600-238">Insertar el servicio de autorización en las vistas</span><span class="sxs-lookup"><span data-stu-id="e5600-238">Inject the authorization service into the views</span></span>

<span data-ttu-id="e5600-239">Actualmente, se muestra en la interfaz de usuario edita y elimina los vínculos de datos que no se puede modificar el usuario.</span><span class="sxs-lookup"><span data-stu-id="e5600-239">Currently, the UI shows edit and delete links for data the user can't modify.</span></span> <span data-ttu-id="e5600-240">La interfaz de usuario es fijo aplicando el controlador de autorización a las vistas.</span><span class="sxs-lookup"><span data-stu-id="e5600-240">The UI is fixed by applying the authorization handler to the views.</span></span>

<span data-ttu-id="e5600-241">Insertar el servicio de autorización en el *Views/_ViewImports.cshtml* para que esté disponible para todas las vistas de archivos:</span><span class="sxs-lookup"><span data-stu-id="e5600-241">Inject the authorization service in the *Views/_ViewImports.cshtml* file so it's available to all views:</span></span>

[!code-cshtml[Main](secure-data/samples/final2/Pages/_ViewImports.cshtml?highlight=6-9)]

<span data-ttu-id="e5600-242">El marcado anterior agrega varias `using` instrucciones.</span><span class="sxs-lookup"><span data-stu-id="e5600-242">The preceding markup adds several `using` statements.</span></span>

<span data-ttu-id="e5600-243">Actualización de la **editar** y **eliminar** vincula *Pages/Contacts/Index.cshtml* por lo que solo se procesan para los usuarios con los permisos adecuados:</span><span class="sxs-lookup"><span data-stu-id="e5600-243">Update the **Edit** and **Delete** links in *Pages/Contacts/Index.cshtml* so they're only rendered for users with the appropriate permissions:</span></span>

[!code-cshtml[Main](secure-data/samples/final2/Pages/Contacts/Index.cshtml?highlight=34-36,64-999)]

> [!WARNING]
> <span data-ttu-id="e5600-244">Ocultar los vínculos de los usuarios que no tienen permiso para cambiar los datos no proteger la aplicación.</span><span class="sxs-lookup"><span data-stu-id="e5600-244">Hiding links from users that don't have permission to change data doesn't secure the app.</span></span> <span data-ttu-id="e5600-245">Ocultar los vínculos hace que la aplicación más fácil de usar mostrando vínculos solo es válido.</span><span class="sxs-lookup"><span data-stu-id="e5600-245">Hiding links makes the app more user-friendly by displaying only valid links.</span></span> <span data-ttu-id="e5600-246">Los usuarios pueden hack las direcciones URL generadas para invocar Editar y eliminar operaciones en los datos que no poseen.</span><span class="sxs-lookup"><span data-stu-id="e5600-246">Users can hack the generated URLs to invoke edit and delete operations on data they don't own.</span></span> <span data-ttu-id="e5600-247">La página de Razor o el controlador debe exigir comprobaciones de acceso para proteger los datos.</span><span class="sxs-lookup"><span data-stu-id="e5600-247">The Razor Page or controller must enforce access checks to secure the data.</span></span>

### <a name="update-details"></a><span data-ttu-id="e5600-248">Detalles de la actualización</span><span class="sxs-lookup"><span data-stu-id="e5600-248">Update Details</span></span>

<span data-ttu-id="e5600-249">Actualice la vista de detalles para que los administradores pueden aprobar o rechazar contactos:</span><span class="sxs-lookup"><span data-stu-id="e5600-249">Update the details view so managers can approve or reject contacts:</span></span>

[!code-cshtml[Main](secure-data/samples/final2/Pages/Contacts/Details.cshtml?range=48-999)]

<span data-ttu-id="e5600-250">Actualice el modelo de páginas de detalles:</span><span class="sxs-lookup"><span data-stu-id="e5600-250">Update the details page model:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="test-the-completed-app"></a><span data-ttu-id="e5600-251">Probar la aplicación completada</span><span class="sxs-lookup"><span data-stu-id="e5600-251">Test the completed app</span></span>

<span data-ttu-id="e5600-252">Si está usando código de Visual Studio o pruebas en una plataforma local que no incluye un certificado de prueba para SSL:</span><span class="sxs-lookup"><span data-stu-id="e5600-252">If you're using Visual Studio Code or testing on a local platform that doesn't include a test certificate for SSL:</span></span>

* <span data-ttu-id="e5600-253">Establecer `"LocalTest:skipSSL": true` en el *appsettings. Developement.JSON* archivo para omitir el requisito de SSL.</span><span class="sxs-lookup"><span data-stu-id="e5600-253">Set `"LocalTest:skipSSL": true` in the *appsettings.Developement.json* file to skip the SSL requirement.</span></span> <span data-ttu-id="e5600-254">Omitir SSL sólo en un equipo de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="e5600-254">Skip SSL only on a development machine.</span></span>

<span data-ttu-id="e5600-255">Si la aplicación tiene contactos:</span><span class="sxs-lookup"><span data-stu-id="e5600-255">If the app has contacts:</span></span>

* <span data-ttu-id="e5600-256">Eliminar todos los registros en la `Contact` tabla.</span><span class="sxs-lookup"><span data-stu-id="e5600-256">Delete all the records in the `Contact` table.</span></span>
* <span data-ttu-id="e5600-257">Reinicie la aplicación para inicializar la base de datos.</span><span class="sxs-lookup"><span data-stu-id="e5600-257">Restart the app to seed the database.</span></span>

<span data-ttu-id="e5600-258">Registrar un usuario para examinar los contactos.</span><span class="sxs-lookup"><span data-stu-id="e5600-258">Register a user for browsing the contacts.</span></span>

<span data-ttu-id="e5600-259">Una manera sencilla de probar la aplicación final consiste en iniciar tres distintos exploradores (o versiones de incógnito/InPrivate).</span><span class="sxs-lookup"><span data-stu-id="e5600-259">An easy way to test the completed app is to launch three different browsers (or incognito/InPrivate versions).</span></span> <span data-ttu-id="e5600-260">En un explorador, registrar un nuevo usuario (por ejemplo, `test@example.com`).</span><span class="sxs-lookup"><span data-stu-id="e5600-260">In one browser, register a new user (for example, `test@example.com`).</span></span> <span data-ttu-id="e5600-261">Inicie sesión con un usuario diferente en cada explorador.</span><span class="sxs-lookup"><span data-stu-id="e5600-261">Sign in to each browser with a different user.</span></span> <span data-ttu-id="e5600-262">Compruebe las operaciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="e5600-262">Verify the following operations:</span></span>

* <span data-ttu-id="e5600-263">Los usuarios registrados pueden ver todos los datos de contacto aprobados.</span><span class="sxs-lookup"><span data-stu-id="e5600-263">Registered users can view all the approved contact data.</span></span>
* <span data-ttu-id="e5600-264">Los usuarios registrados pueden editar o eliminar sus propios datos.</span><span class="sxs-lookup"><span data-stu-id="e5600-264">Registered users can edit/delete their own data.</span></span>
* <span data-ttu-id="e5600-265">Los administradores pueden aprobar o rechazar datos de contacto.</span><span class="sxs-lookup"><span data-stu-id="e5600-265">Managers can approve or reject contact data.</span></span> <span data-ttu-id="e5600-266">El `Details` vista muestra **aprobar** y **rechazar** botones.</span><span class="sxs-lookup"><span data-stu-id="e5600-266">The `Details` view shows **Approve** and **Reject** buttons.</span></span>
* <span data-ttu-id="e5600-267">Los administradores pueden aprobar o rechazar y editar o eliminar los datos.</span><span class="sxs-lookup"><span data-stu-id="e5600-267">Administrators can approve/reject and edit/delete any data.</span></span>

| <span data-ttu-id="e5600-268">Usuario</span><span class="sxs-lookup"><span data-stu-id="e5600-268">User</span></span>| <span data-ttu-id="e5600-269">Opciones</span><span class="sxs-lookup"><span data-stu-id="e5600-269">Options</span></span> |
| ------------ | ---------|
| test@example.com | <span data-ttu-id="e5600-270">Editar puede/eliminar datos propios</span><span class="sxs-lookup"><span data-stu-id="e5600-270">Can edit/delete own data</span></span> |
| manager@contoso.com | <span data-ttu-id="e5600-271">Pueden aprobar o rechazar y editar o eliminar datos de propietario</span><span class="sxs-lookup"><span data-stu-id="e5600-271">Can approve/reject and edit/delete own data</span></span> |
| admin@contoso.com | <span data-ttu-id="e5600-272">Puede editar o eliminar y aprobar o rechazar todos los datos</span><span class="sxs-lookup"><span data-stu-id="e5600-272">Can edit/delete and approve/reject all data</span></span>|

<span data-ttu-id="e5600-273">Crear un contacto en el explorador del administrador.</span><span class="sxs-lookup"><span data-stu-id="e5600-273">Create a contact in the administrator's browser.</span></span> <span data-ttu-id="e5600-274">Copie la dirección URL para eliminar y editar en el contacto del administrador.</span><span class="sxs-lookup"><span data-stu-id="e5600-274">Copy the URL for delete and edit from the administrator contact.</span></span> <span data-ttu-id="e5600-275">Pegue estos vínculos en el explorador del usuario de prueba para comprobar que el usuario de prueba no puede realizar estas operaciones.</span><span class="sxs-lookup"><span data-stu-id="e5600-275">Paste these links into the test user's browser to verify the test user can't perform these operations.</span></span>

## <a name="create-the-starter-app"></a><span data-ttu-id="e5600-276">Crear la aplicación de inicio</span><span class="sxs-lookup"><span data-stu-id="e5600-276">Create the starter app</span></span>

* <span data-ttu-id="e5600-277">Crear una aplicación de páginas de Razor denominada "ContactManager"</span><span class="sxs-lookup"><span data-stu-id="e5600-277">Create a Razor Pages app named "ContactManager"</span></span>

  * <span data-ttu-id="e5600-278">Crear la aplicación con **cuentas de usuario individuales**.</span><span class="sxs-lookup"><span data-stu-id="e5600-278">Create the app with **Individual User Accounts**.</span></span>
  * <span data-ttu-id="e5600-279">Asígnele el nombre "ContactManager" para el espacio de nombres coincide con el espacio de nombres utilizado en el ejemplo.</span><span class="sxs-lookup"><span data-stu-id="e5600-279">Name it "ContactManager" so your namespace matches the namespace used in the sample.</span></span>

  ```console
  dotnet new razor -o ContactManager -au Individual -uld
  ```

  * <span data-ttu-id="e5600-280">`-uld`Especifica LocalDB en lugar de SQLite</span><span class="sxs-lookup"><span data-stu-id="e5600-280">`-uld` specifies LocalDB instead of SQLite</span></span>

* <span data-ttu-id="e5600-281">Agregue las siguientes `Contact` modelo:</span><span class="sxs-lookup"><span data-stu-id="e5600-281">Add the following `Contact` model:</span></span>

  [!code-csharp[Main](secure-data/samples/starter2/Models/Contact.cs?name=snippet1)]

* <span data-ttu-id="e5600-282">Scaffold el `Contact` modelo:</span><span class="sxs-lookup"><span data-stu-id="e5600-282">Scaffold the `Contact` model:</span></span>

```console
dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
```

* <span data-ttu-id="e5600-283">Actualización de la **ContactManager** fijar en el *Pages/_Layout.cshtml* archivo:</span><span class="sxs-lookup"><span data-stu-id="e5600-283">Update the **ContactManager** anchor in the *Pages/_Layout.cshtml* file:</span></span>

```cshtml
<a asp-page="/Contacts/Index" class="navbar-brand">ContactManager</a>
```

* <span data-ttu-id="e5600-284">Aplicar la técnica scaffolding la migración inicial y actualizar la base de datos:</span><span class="sxs-lookup"><span data-stu-id="e5600-284">Scaffold the initial migration and update the database:</span></span>

```console
dotnet ef migrations add initial
dotnet ef database update
```

* <span data-ttu-id="e5600-285">Probar la aplicación mediante la creación, edición y eliminación de un contacto</span><span class="sxs-lookup"><span data-stu-id="e5600-285">Test the app by creating, editing, and deleting a contact</span></span>

### <a name="seed-the-database"></a><span data-ttu-id="e5600-286">Inicializar la base de datos</span><span class="sxs-lookup"><span data-stu-id="e5600-286">Seed the database</span></span>

<span data-ttu-id="e5600-287">Agregar el `SeedData` clase a la *datos* carpeta.</span><span class="sxs-lookup"><span data-stu-id="e5600-287">Add the `SeedData` class to the *Data* folder.</span></span> <span data-ttu-id="e5600-288">Si ha descargado el ejemplo, puede copiar la *SeedData.cs* del archivo a la *datos* carpeta del proyecto de inicio.</span><span class="sxs-lookup"><span data-stu-id="e5600-288">If you've downloaded the sample, you can copy the *SeedData.cs* file to the *Data* folder of the starter project.</span></span>

<span data-ttu-id="e5600-289">Llame a `SeedData.Initialize` de `Main`:</span><span class="sxs-lookup"><span data-stu-id="e5600-289">Call `SeedData.Initialize` from `Main`:</span></span>

[!code-csharp[Main](secure-data/samples/starter2/Program.cs?name=snippet)]

<span data-ttu-id="e5600-290">Compruebe que la aplicación había propagado la base de datos.</span><span class="sxs-lookup"><span data-stu-id="e5600-290">Test that the app seeded the database.</span></span> <span data-ttu-id="e5600-291">Si no hay ninguna fila en la base de datos de contacto, no se ejecuta el método de inicialización.</span><span class="sxs-lookup"><span data-stu-id="e5600-291">If there are any rows in the contact DB, the seed method doesn't run.</span></span>

<a name="secure-data-add-resources-label"></a>

### <a name="additional-resources"></a><span data-ttu-id="e5600-292">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="e5600-292">Additional resources</span></span>

* <span data-ttu-id="e5600-293">[Laboratorio de autorización de ASP.NET Core](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span><span class="sxs-lookup"><span data-stu-id="e5600-293">[ASP.NET Core Authorization Lab](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span></span> <span data-ttu-id="e5600-294">Este laboratorio se explica con más detalle en las características de seguridad introducidas en este tutorial.</span><span class="sxs-lookup"><span data-stu-id="e5600-294">This lab goes into more detail on the security features introduced in this tutorial.</span></span>
* [<span data-ttu-id="e5600-295">Autorización de ASP.NET Core: Simple, rol, basada en notificaciones y personalizado</span><span class="sxs-lookup"><span data-stu-id="e5600-295">Authorization in ASP.NET Core: Simple, role, claims-based, and custom</span></span>](xref:security/authorization/index)
* [<span data-ttu-id="e5600-296">Autorización personalizada basada en directivas</span><span class="sxs-lookup"><span data-stu-id="e5600-296">Custom policy-based authorization</span></span>](xref:security/authorization/policies)
