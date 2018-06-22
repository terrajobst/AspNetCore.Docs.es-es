---
title: Crear una aplicación de ASP.NET Core con datos de usuario protegidos por autorización
author: rick-anderson
description: Obtenga información acerca de cómo crear una aplicación de páginas de Razor con datos de usuario protegidos mediante autorización. Incluye HTTPS, la autenticación, la seguridad, ASP.NET Core Identity.
ms.author: riande
ms.date: 01/24/2018
uid: security/authorization/secure-data
ms.openlocfilehash: 32ced054ba559e66de4a300137d56b7c81a4149b
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276385"
---
# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a><span data-ttu-id="d09b8-104">Crear una aplicación de ASP.NET Core con datos de usuario protegidos por autorización</span><span class="sxs-lookup"><span data-stu-id="d09b8-104">Create an ASP.NET Core app with user data protected by authorization</span></span>

<span data-ttu-id="d09b8-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT) y [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="d09b8-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="d09b8-106">Este tutorial muestra cómo crear una aplicación web de ASP.NET Core con datos de usuario protegidos mediante autorización.</span><span class="sxs-lookup"><span data-stu-id="d09b8-106">This tutorial shows how to create an ASP.NET Core web app with user data protected by authorization.</span></span> <span data-ttu-id="d09b8-107">Muestra una lista de contactos que autentica los usuarios (registrados) ha creado.</span><span class="sxs-lookup"><span data-stu-id="d09b8-107">It displays a list of contacts that authenticated (registered) users have created.</span></span> <span data-ttu-id="d09b8-108">Hay tres grupos de seguridad:</span><span class="sxs-lookup"><span data-stu-id="d09b8-108">There are three security groups:</span></span>

* <span data-ttu-id="d09b8-109">**Usuarios registrados** puede ver todos los datos aprobados y editar puede/eliminar sus propios datos.</span><span class="sxs-lookup"><span data-stu-id="d09b8-109">**Registered users** can view all the approved data and can edit/delete their own data.</span></span>
* <span data-ttu-id="d09b8-110">**Administradores de** puede aprobar o rechazar los datos de contacto.</span><span class="sxs-lookup"><span data-stu-id="d09b8-110">**Managers** can approve or reject contact data.</span></span> <span data-ttu-id="d09b8-111">Sólo los contactos aprobados son visibles para los usuarios.</span><span class="sxs-lookup"><span data-stu-id="d09b8-111">Only approved contacts are visible to users.</span></span>
* <span data-ttu-id="d09b8-112">**Los administradores** puede aprobar o rechazar y editar o eliminar los datos.</span><span class="sxs-lookup"><span data-stu-id="d09b8-112">**Administrators** can approve/reject and edit/delete any data.</span></span>

<span data-ttu-id="d09b8-113">En la siguiente imagen, usuario Rick (`rick@example.com`) ha iniciado sesión.</span><span class="sxs-lookup"><span data-stu-id="d09b8-113">In the following image, user Rick (`rick@example.com`) is signed in.</span></span> <span data-ttu-id="d09b8-114">Rick solo puede ver los contactos aprobados y **editar**/**eliminar**/**crear nuevo** vínculos de sus contactos.</span><span class="sxs-lookup"><span data-stu-id="d09b8-114">Rick can only view approved contacts and **Edit**/**Delete**/**Create New** links for his contacts.</span></span> <span data-ttu-id="d09b8-115">El último registro, creado por Rick, muestra **editar** y **eliminar** vínculos.</span><span class="sxs-lookup"><span data-stu-id="d09b8-115">Only the last record, created by Rick, displays **Edit** and **Delete** links.</span></span> <span data-ttu-id="d09b8-116">Otros usuarios no verán el último registro hasta que un administrador o un administrador cambia el estado a "Aprobado".</span><span class="sxs-lookup"><span data-stu-id="d09b8-116">Other users won't see the last record until a manager or administrator changes the status to "Approved".</span></span>

![imagen describe anterior](secure-data/_static/rick.png)

<span data-ttu-id="d09b8-118">En la siguiente imagen, `manager@contoso.com` está firmado en y en el rol de administrador:</span><span class="sxs-lookup"><span data-stu-id="d09b8-118">In the following image, `manager@contoso.com` is signed in and in the managers role:</span></span>

![imagen describe anterior](secure-data/_static/manager1.png)

<span data-ttu-id="d09b8-120">La siguiente imagen muestra a los administradores de la vista de detalles de un contacto:</span><span class="sxs-lookup"><span data-stu-id="d09b8-120">The following image shows the managers details view of a contact:</span></span>

![imagen describe anterior](secure-data/_static/manager.png)

<span data-ttu-id="d09b8-122">El **aprobar** y **rechazar** solo se muestran los botones para los administradores y administradores.</span><span class="sxs-lookup"><span data-stu-id="d09b8-122">The **Approve** and **Reject** buttons are only displayed for managers and administrators.</span></span>

<span data-ttu-id="d09b8-123">En la siguiente imagen, `admin@contoso.com` está firmado en y en la función Administradores:</span><span class="sxs-lookup"><span data-stu-id="d09b8-123">In the following image, `admin@contoso.com` is signed in and in the administrators role:</span></span>

![imagen describe anterior](secure-data/_static/admin.png)

<span data-ttu-id="d09b8-125">El administrador tiene todos los privilegios.</span><span class="sxs-lookup"><span data-stu-id="d09b8-125">The administrator has all privileges.</span></span> <span data-ttu-id="d09b8-126">Puede leer, editar o eliminar cualquier contacto y cambiar el estado de contactos.</span><span class="sxs-lookup"><span data-stu-id="d09b8-126">She can read/edit/delete any contact and change the status of contacts.</span></span>

<span data-ttu-id="d09b8-127">La aplicación se creó con [scaffolding](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller) siguiente `Contact` modelo:</span><span class="sxs-lookup"><span data-stu-id="d09b8-127">The app was created by [scaffolding](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller) the following `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/starter2/Models/Contact.cs?name=snippet1)]

<span data-ttu-id="d09b8-128">El ejemplo contiene los siguientes controladores de autorización:</span><span class="sxs-lookup"><span data-stu-id="d09b8-128">The sample contains the following authorization handlers:</span></span>

* <span data-ttu-id="d09b8-129">`ContactIsOwnerAuthorizationHandler`: Se asegura de que un usuario solo puede editar sus datos.</span><span class="sxs-lookup"><span data-stu-id="d09b8-129">`ContactIsOwnerAuthorizationHandler`: Ensures that a user can only edit their data.</span></span>
* <span data-ttu-id="d09b8-130">`ContactManagerAuthorizationHandler`: Permite a los administradores aprobar o rechazar los contactos.</span><span class="sxs-lookup"><span data-stu-id="d09b8-130">`ContactManagerAuthorizationHandler`: Allows managers to approve or reject contacts.</span></span>
* <span data-ttu-id="d09b8-131">`ContactAdministratorsAuthorizationHandler`: Permite a los administradores para aprobar o rechazar los contactos y editar o eliminar contactos.</span><span class="sxs-lookup"><span data-stu-id="d09b8-131">`ContactAdministratorsAuthorizationHandler`: Allows administrators to approve or reject contacts and to edit/delete contacts.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d09b8-132">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="d09b8-132">Prerequisites</span></span>

<span data-ttu-id="d09b8-133">Este tutorial se avanza.</span><span class="sxs-lookup"><span data-stu-id="d09b8-133">This tutorial is advanced.</span></span> <span data-ttu-id="d09b8-134">Debe estar familiarizado con:</span><span class="sxs-lookup"><span data-stu-id="d09b8-134">You should be familiar with:</span></span>

* [<span data-ttu-id="d09b8-135">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d09b8-135">ASP.NET Core</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="d09b8-136">Autenticación</span><span class="sxs-lookup"><span data-stu-id="d09b8-136">Authentication</span></span>](xref:security/authentication/index)
* [<span data-ttu-id="d09b8-137">Confirmación de cuentas y recuperación de contraseñas</span><span class="sxs-lookup"><span data-stu-id="d09b8-137">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="d09b8-138">Autorización</span><span class="sxs-lookup"><span data-stu-id="d09b8-138">Authorization</span></span>](xref:security/authorization/index)
* [<span data-ttu-id="d09b8-139">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="d09b8-139">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

<span data-ttu-id="d09b8-140">Vea [este archivo PDF](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) para la versión principal de ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="d09b8-140">See [this PDF file](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core MVC version.</span></span> <span data-ttu-id="d09b8-141">La versión 1.1 de ASP.NET Core de este tutorial está en [esto](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data) carpeta.</span><span class="sxs-lookup"><span data-stu-id="d09b8-141">The ASP.NET Core 1.1 version of this tutorial is in [this](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data) folder.</span></span> <span data-ttu-id="d09b8-142">La 1.1 incluye el ejemplo de ASP.NET Core el [ejemplos](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).</span><span class="sxs-lookup"><span data-stu-id="d09b8-142">The 1.1 ASP.NET Core sample is in the [samples](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).</span></span>

## <a name="the-starter-and-completed-app"></a><span data-ttu-id="d09b8-143">El inicio y la aplicación completada</span><span class="sxs-lookup"><span data-stu-id="d09b8-143">The starter and completed app</span></span>

<span data-ttu-id="d09b8-144">[Descargar](xref:tutorials/index#how-to-download-a-sample) el [completado](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2) aplicación.</span><span class="sxs-lookup"><span data-stu-id="d09b8-144">[Download](xref:tutorials/index#how-to-download-a-sample) the [completed](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2) app.</span></span> <span data-ttu-id="d09b8-145">[Prueba](#test-the-completed-app) la aplicación completada por lo que se familiarice con sus características de seguridad.</span><span class="sxs-lookup"><span data-stu-id="d09b8-145">[Test](#test-the-completed-app) the completed app so you become familiar with its security features.</span></span>

### <a name="the-starter-app"></a><span data-ttu-id="d09b8-146">La aplicación de inicio</span><span class="sxs-lookup"><span data-stu-id="d09b8-146">The starter app</span></span>

<span data-ttu-id="d09b8-147">[Descargar](xref:tutorials/index#how-to-download-a-sample) el [starter](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2) aplicación.</span><span class="sxs-lookup"><span data-stu-id="d09b8-147">[Download](xref:tutorials/index#how-to-download-a-sample) the [starter](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2) app.</span></span>

<span data-ttu-id="d09b8-148">Ejecutar la aplicación, pulse el **ContactManager** vincular y compruebe que puede crear, editar y eliminar un contacto.</span><span class="sxs-lookup"><span data-stu-id="d09b8-148">Run the app, tap the **ContactManager** link, and verify you can create, edit, and delete a contact.</span></span>

## <a name="secure-user-data"></a><span data-ttu-id="d09b8-149">Proteger los datos de usuario</span><span class="sxs-lookup"><span data-stu-id="d09b8-149">Secure user data</span></span>

<span data-ttu-id="d09b8-150">Las siguientes secciones contienen todos los pasos principales para crear la aplicación de datos de usuario seguras.</span><span class="sxs-lookup"><span data-stu-id="d09b8-150">The following sections have all the major steps to create the secure user data app.</span></span> <span data-ttu-id="d09b8-151">Le resultará útil para hacer referencia al proyecto completado.</span><span class="sxs-lookup"><span data-stu-id="d09b8-151">You may find it helpful to refer to the completed project.</span></span>

### <a name="tie-the-contact-data-to-the-user"></a><span data-ttu-id="d09b8-152">Asociar los datos de contacto para el usuario</span><span class="sxs-lookup"><span data-stu-id="d09b8-152">Tie the contact data to the user</span></span>

<span data-ttu-id="d09b8-153">Usar ASP.NET [identidad](xref:security/authentication/identity) Id. de usuario para garantizar que los usuarios puede editar sus datos, pero no otros datos de los usuarios.</span><span class="sxs-lookup"><span data-stu-id="d09b8-153">Use the ASP.NET [Identity](xref:security/authentication/identity) user ID to ensure users can edit their data, but not other users data.</span></span> <span data-ttu-id="d09b8-154">Agregar `OwnerID` y `ContactStatus` a la `Contact` modelo:</span><span class="sxs-lookup"><span data-stu-id="d09b8-154">Add `OwnerID` and `ContactStatus` to the `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/final2/Models/Contact.cs?name=snippet1&highlight=5-6,16-999)]

<span data-ttu-id="d09b8-155">`OwnerID` es el identificador del usuario desde el `AspNetUser` tabla el [identidad](xref:security/authentication/identity) base de datos.</span><span class="sxs-lookup"><span data-stu-id="d09b8-155">`OwnerID` is the user's ID from the `AspNetUser` table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="d09b8-156">El `Status` campo determina si un contacto es visible para los usuarios en general.</span><span class="sxs-lookup"><span data-stu-id="d09b8-156">The `Status` field determines if a contact is viewable by general users.</span></span>

<span data-ttu-id="d09b8-157">Crear una nueva migración y actualizar la base de datos:</span><span class="sxs-lookup"><span data-stu-id="d09b8-157">Create a new migration and update the database:</span></span>

```console
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="require-https-and-authenticated-users"></a><span data-ttu-id="d09b8-158">Requerir HTTPS y los usuarios autenticados</span><span class="sxs-lookup"><span data-stu-id="d09b8-158">Require HTTPS and authenticated users</span></span>

<span data-ttu-id="d09b8-159">Agregar [IHostingEnvironment](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) a `Startup`:</span><span class="sxs-lookup"><span data-stu-id="d09b8-159">Add [IHostingEnvironment](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) to `Startup`:</span></span>

[!code-csharp[](secure-data/samples/final2/Startup.cs?name=snippet_env)]

<span data-ttu-id="d09b8-160">En el `ConfigureServices` método de la *Startup.cs* , agregue el [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) filtro de autorización:</span><span class="sxs-lookup"><span data-stu-id="d09b8-160">In the `ConfigureServices` method of the *Startup.cs* file, add the [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) authorization filter:</span></span>

[!code-csharp[](secure-data/samples/final2/Startup.cs?name=snippet_SSL&highlight=10-999)]

<span data-ttu-id="d09b8-161">Si está utilizando Visual Studio, habilitar HTTPS.</span><span class="sxs-lookup"><span data-stu-id="d09b8-161">If you're using Visual Studio, enable HTTPS.</span></span>

<span data-ttu-id="d09b8-162">Para redirigir las solicitudes HTTP a HTTPS, consulte [Middleware de reescritura de dirección URL](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="d09b8-162">To redirect HTTP requests to HTTPS, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span> <span data-ttu-id="d09b8-163">Si está usando Visual Studio Code o pruebas en una plataforma local que no incluye un certificado de prueba para HTTPS:</span><span class="sxs-lookup"><span data-stu-id="d09b8-163">If you're using Visual Studio Code or testing on a local platform that doesn't include a test certificate for HTTPS:</span></span>

  <span data-ttu-id="d09b8-164">Establecer `"LocalTest:skipHTTPS": true` en el *appsettings. Developement.JSON* archivo.</span><span class="sxs-lookup"><span data-stu-id="d09b8-164">Set `"LocalTest:skipHTTPS": true` in the *appsettings.Developement.json* file.</span></span>

### <a name="require-authenticated-users"></a><span data-ttu-id="d09b8-165">Requerir a los usuarios autenticados</span><span class="sxs-lookup"><span data-stu-id="d09b8-165">Require authenticated users</span></span>

<span data-ttu-id="d09b8-166">Establecer la directiva de autenticación predeterminado para exigir que los usuarios se autentiquen.</span><span class="sxs-lookup"><span data-stu-id="d09b8-166">Set the default authentication policy to require users to be authenticated.</span></span> <span data-ttu-id="d09b8-167">Puede rechazar la autenticación en el nivel de método página Razor, controlador o acción con el `[AllowAnonymous]` atributo.</span><span class="sxs-lookup"><span data-stu-id="d09b8-167">You can opt out of authentication at the Razor Page, controller, or action method level with the `[AllowAnonymous]` attribute.</span></span> <span data-ttu-id="d09b8-168">Establecer la directiva de autenticación predeterminado para exigir que los usuarios se autentiquen protege recién agregado las páginas Razor y controladores.</span><span class="sxs-lookup"><span data-stu-id="d09b8-168">Setting the default authentication policy to require users to be authenticated protects newly added Razor Pages and controllers.</span></span> <span data-ttu-id="d09b8-169">Existencia de autenticación que requiere de forma predeterminada es más segura que confiar en los nuevos controladores y las páginas de Razor para incluir la `[Authorize]` atributo.</span><span class="sxs-lookup"><span data-stu-id="d09b8-169">Having authentication required by default is safer than relying on new controllers and Razor Pages to include the `[Authorize]` attribute.</span></span> 

<span data-ttu-id="d09b8-170">Con el requisito de todos los usuarios autenticados, el [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder?view=aspnetcore-2.0#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) y [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage?view=aspnetcore-2.0) llamadas no son necesarias.</span><span class="sxs-lookup"><span data-stu-id="d09b8-170">With the requirement of all users authenticated, the [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder?view=aspnetcore-2.0#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) and [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage?view=aspnetcore-2.0) calls are not required.</span></span>

<span data-ttu-id="d09b8-171">Actualización `ConfigureServices` con los cambios siguientes:</span><span class="sxs-lookup"><span data-stu-id="d09b8-171">Update `ConfigureServices` with the following changes:</span></span>

* <span data-ttu-id="d09b8-172">Convertir en comentario `AuthorizeFolder` y `AuthorizePage`.</span><span class="sxs-lookup"><span data-stu-id="d09b8-172">Comment out `AuthorizeFolder` and `AuthorizePage`.</span></span>
* <span data-ttu-id="d09b8-173">Establecer la directiva de autenticación predeterminado para exigir que los usuarios se autentiquen.</span><span class="sxs-lookup"><span data-stu-id="d09b8-173">Set the default authentication policy to require users to be authenticated.</span></span>

[!code-csharp[](secure-data/samples/final2/Startup.cs?name=snippet_defaultPolicy&highlight=23-27,31-999)]

<span data-ttu-id="d09b8-174">Agregar [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) al índice, las páginas sobre y, a continuación, póngase en contacto con por lo que los usuarios anónimos pueden recibir información sobre el sitio antes de que registrar.</span><span class="sxs-lookup"><span data-stu-id="d09b8-174">Add [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) to the Index, About, and Contact pages so anonymous users can get information about the site before they register.</span></span> 

[!code-csharp[](secure-data/samples/final2/Pages/Index.cshtml.cs?name=snippet&highlight=2)]

<span data-ttu-id="d09b8-175">Agregar `[AllowAnonymous]` a la [LoginModel y RegisterModel](https://github.com/aspnet/templating/issues/238).</span><span class="sxs-lookup"><span data-stu-id="d09b8-175">Add `[AllowAnonymous]` to the [LoginModel and RegisterModel](https://github.com/aspnet/templating/issues/238).</span></span>

### <a name="configure-the-test-account"></a><span data-ttu-id="d09b8-176">Configurar la cuenta de prueba</span><span class="sxs-lookup"><span data-stu-id="d09b8-176">Configure the test account</span></span>

<span data-ttu-id="d09b8-177">La `SeedData` clase crea dos cuentas: administrador y el administrador.</span><span class="sxs-lookup"><span data-stu-id="d09b8-177">The `SeedData` class creates two accounts: administrator and manager.</span></span> <span data-ttu-id="d09b8-178">Use la [herramienta secreto administrador](xref:security/app-secrets) para establecer una contraseña para estas cuentas.</span><span class="sxs-lookup"><span data-stu-id="d09b8-178">Use the [Secret Manager tool](xref:security/app-secrets) to set a password for these accounts.</span></span> <span data-ttu-id="d09b8-179">Establecer la contraseña desde el directorio del proyecto (el directorio que contiene *Program.cs*):</span><span class="sxs-lookup"><span data-stu-id="d09b8-179">Set the password from the project directory (the directory containing *Program.cs*):</span></span>

```console
dotnet user-secrets set SeedUserPW <PW>
```

<span data-ttu-id="d09b8-180">Si no utiliza una contraseña segura, se produce una excepción cuando `SeedData.Initialize` se llama.</span><span class="sxs-lookup"><span data-stu-id="d09b8-180">If you don't use a strong password, an exception is thrown when `SeedData.Initialize` is called.</span></span>

<span data-ttu-id="d09b8-181">Actualización `Main` usar la contraseña de prueba:</span><span class="sxs-lookup"><span data-stu-id="d09b8-181">Update `Main` to use the test password:</span></span>

[!code-csharp[](secure-data/samples/final2/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a><span data-ttu-id="d09b8-182">Crear las cuentas de prueba y actualizar los contactos</span><span class="sxs-lookup"><span data-stu-id="d09b8-182">Create the test accounts and update the contacts</span></span>

<span data-ttu-id="d09b8-183">Actualización de la `Initialize` método en la `SeedData` clase para crear las cuentas de prueba:</span><span class="sxs-lookup"><span data-stu-id="d09b8-183">Update the `Initialize` method in the `SeedData` class to create the test accounts:</span></span>

[!code-csharp[](secure-data/samples/final2/Data/SeedData.cs?name=snippet_Initialize)]

<span data-ttu-id="d09b8-184">Agregue el identificador de usuario de administrador y `ContactStatus` a los contactos.</span><span class="sxs-lookup"><span data-stu-id="d09b8-184">Add the administrator user ID and `ContactStatus` to the contacts.</span></span> <span data-ttu-id="d09b8-185">Realice uno de los contactos "Enviado" y un "rechazada".</span><span class="sxs-lookup"><span data-stu-id="d09b8-185">Make one of the contacts "Submitted" and one "Rejected".</span></span> <span data-ttu-id="d09b8-186">Agregue el Id. de usuario y el estado para todos los contactos.</span><span class="sxs-lookup"><span data-stu-id="d09b8-186">Add the user ID and status to all the contacts.</span></span> <span data-ttu-id="d09b8-187">Póngase en contacto un solo con se muestra:</span><span class="sxs-lookup"><span data-stu-id="d09b8-187">Only one contact is shown:</span></span>

[!code-csharp[](secure-data/samples/final2/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a><span data-ttu-id="d09b8-188">Crear propietario, el administrador y los controladores de autorización de administrador</span><span class="sxs-lookup"><span data-stu-id="d09b8-188">Create owner, manager, and administrator authorization handlers</span></span>

<span data-ttu-id="d09b8-189">Crear un `ContactIsOwnerAuthorizationHandler` clase en el *autorización* carpeta.</span><span class="sxs-lookup"><span data-stu-id="d09b8-189">Create a `ContactIsOwnerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="d09b8-190">El `ContactIsOwnerAuthorizationHandler` comprueba que el usuario que actúa en un recurso posee el recurso.</span><span class="sxs-lookup"><span data-stu-id="d09b8-190">The `ContactIsOwnerAuthorizationHandler` verifies that the user acting on a resource owns the resource.</span></span>

[!code-csharp[](secure-data/samples/final2/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

<span data-ttu-id="d09b8-191">El `ContactIsOwnerAuthorizationHandler` llamadas [contexto. Correctamente](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) si el usuario autenticado actual es el propietario del contacto.</span><span class="sxs-lookup"><span data-stu-id="d09b8-191">The `ContactIsOwnerAuthorizationHandler` calls [context.Succeed](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) if the current authenticated user is the contact owner.</span></span> <span data-ttu-id="d09b8-192">Controladores de autorización general:</span><span class="sxs-lookup"><span data-stu-id="d09b8-192">Authorization handlers generally:</span></span>

* <span data-ttu-id="d09b8-193">Devolver `context.Succeed` cuando se cumplen los requisitos.</span><span class="sxs-lookup"><span data-stu-id="d09b8-193">Return `context.Succeed` when the requirements are met.</span></span>
* <span data-ttu-id="d09b8-194">Devolver `Task.CompletedTask` cuando no se cumplen los requisitos.</span><span class="sxs-lookup"><span data-stu-id="d09b8-194">Return `Task.CompletedTask` when requirements aren't met.</span></span> <span data-ttu-id="d09b8-195">`Task.CompletedTask` no correcto o error&mdash;permite que otros controladores de autorización para que se ejecute.</span><span class="sxs-lookup"><span data-stu-id="d09b8-195">`Task.CompletedTask` is neither success or failure&mdash;it allows other authorization handlers to run.</span></span>

<span data-ttu-id="d09b8-196">Si necesita explícitamente un error, devolver [contexto. Un error](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span><span class="sxs-lookup"><span data-stu-id="d09b8-196">If you need to explicitly fail, return [context.Fail](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span></span>

<span data-ttu-id="d09b8-197">La aplicación permite a los propietarios de contacto para editar, eliminar o crear sus propios datos.</span><span class="sxs-lookup"><span data-stu-id="d09b8-197">The app allows contact owners to edit/delete/create their own data.</span></span> <span data-ttu-id="d09b8-198">`ContactIsOwnerAuthorizationHandler` no tiene que comprobar la operación pasada en el parámetro de requisito.</span><span class="sxs-lookup"><span data-stu-id="d09b8-198">`ContactIsOwnerAuthorizationHandler` doesn't need to check the operation passed in the requirement parameter.</span></span>

### <a name="create-a-manager-authorization-handler"></a><span data-ttu-id="d09b8-199">Crear un controlador del Administrador de autorización</span><span class="sxs-lookup"><span data-stu-id="d09b8-199">Create a manager authorization handler</span></span>

<span data-ttu-id="d09b8-200">Crear un `ContactManagerAuthorizationHandler` clase en el *autorización* carpeta.</span><span class="sxs-lookup"><span data-stu-id="d09b8-200">Create a `ContactManagerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="d09b8-201">El `ContactManagerAuthorizationHandler` comprueba que el usuario que actúa en el recurso es un administrador.</span><span class="sxs-lookup"><span data-stu-id="d09b8-201">The `ContactManagerAuthorizationHandler` verifies the user acting on the resource is a manager.</span></span> <span data-ttu-id="d09b8-202">Solo los administradores pueden aprobar o rechazar los cambios de contenido (nuevos o modificados).</span><span class="sxs-lookup"><span data-stu-id="d09b8-202">Only managers can approve or reject content changes (new or changed).</span></span>

[!code-csharp[](secure-data/samples/final2/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a><span data-ttu-id="d09b8-203">Crear un controlador de autorización de administrador</span><span class="sxs-lookup"><span data-stu-id="d09b8-203">Create an administrator authorization handler</span></span>

<span data-ttu-id="d09b8-204">Crear un `ContactAdministratorsAuthorizationHandler` clase en el *autorización* carpeta.</span><span class="sxs-lookup"><span data-stu-id="d09b8-204">Create a `ContactAdministratorsAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="d09b8-205">El `ContactAdministratorsAuthorizationHandler` comprueba que el usuario que actúa en el recurso es un administrador.</span><span class="sxs-lookup"><span data-stu-id="d09b8-205">The `ContactAdministratorsAuthorizationHandler` verifies the user acting on the resource is an administrator.</span></span> <span data-ttu-id="d09b8-206">Administrador puede hacer que todas las operaciones.</span><span class="sxs-lookup"><span data-stu-id="d09b8-206">Administrator can do all operations.</span></span>

[!code-csharp[](secure-data/samples/final2/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a><span data-ttu-id="d09b8-207">Registrar los controladores de autorización</span><span class="sxs-lookup"><span data-stu-id="d09b8-207">Register the authorization handlers</span></span>

<span data-ttu-id="d09b8-208">Servicios mediante Entity Framework Core deben estar registrados para [inyección de dependencia](xref:fundamentals/dependency-injection) con [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span><span class="sxs-lookup"><span data-stu-id="d09b8-208">Services using Entity Framework Core must be registered for [dependency injection](xref:fundamentals/dependency-injection) using [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span></span> <span data-ttu-id="d09b8-209">El `ContactIsOwnerAuthorizationHandler` usa ASP.NET Core [identidad](xref:security/authentication/identity), que se basa en Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="d09b8-209">The `ContactIsOwnerAuthorizationHandler` uses ASP.NET Core [Identity](xref:security/authentication/identity), which is built on Entity Framework Core.</span></span> <span data-ttu-id="d09b8-210">Registre los controladores con la colección de servicio para que estén disponibles para la `ContactsController` a través de [inyección de dependencia](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="d09b8-210">Register the handlers with the service collection so they're available to the `ContactsController` through [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="d09b8-211">Agregue el código siguiente al final de `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="d09b8-211">Add the following code to the end of `ConfigureServices`:</span></span>

[!code-csharp[](secure-data/samples/final2/Startup.cs?name=ConfigureServices&highlight=41-999)]

<span data-ttu-id="d09b8-212">`ContactAdministratorsAuthorizationHandler` y `ContactManagerAuthorizationHandler` se agregan como singleton.</span><span class="sxs-lookup"><span data-stu-id="d09b8-212">`ContactAdministratorsAuthorizationHandler` and `ContactManagerAuthorizationHandler` are added as singletons.</span></span> <span data-ttu-id="d09b8-213">Son singletons porque no usan EF y toda la información necesaria se encuentra en la `Context` parámetro de la `HandleRequirementAsync` método.</span><span class="sxs-lookup"><span data-stu-id="d09b8-213">They're singletons because they don't use EF and all the information needed is in the `Context` parameter of the `HandleRequirementAsync` method.</span></span>

## <a name="support-authorization"></a><span data-ttu-id="d09b8-214">Admitir la autorización</span><span class="sxs-lookup"><span data-stu-id="d09b8-214">Support authorization</span></span>

<span data-ttu-id="d09b8-215">En esta sección, actualice las páginas Razor y agregar una clase de requisitos de las operaciones.</span><span class="sxs-lookup"><span data-stu-id="d09b8-215">In this section, you update the Razor Pages and add an operations requirements class.</span></span>

### <a name="review-the-contact-operations-requirements-class"></a><span data-ttu-id="d09b8-216">Revisión de la clase de requisitos de operaciones de contacto</span><span class="sxs-lookup"><span data-stu-id="d09b8-216">Review the contact operations requirements class</span></span>

<span data-ttu-id="d09b8-217">Revise la `ContactOperations` clase.</span><span class="sxs-lookup"><span data-stu-id="d09b8-217">Review the `ContactOperations` class.</span></span> <span data-ttu-id="d09b8-218">Esta clase contiene los requisitos de la aplicación admite:</span><span class="sxs-lookup"><span data-stu-id="d09b8-218">This class contains the requirements the app supports:</span></span>

[!code-csharp[](secure-data/samples/final2/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-razor-pages"></a><span data-ttu-id="d09b8-219">Crear una clase base para las páginas de Razor</span><span class="sxs-lookup"><span data-stu-id="d09b8-219">Create a base class for the Razor Pages</span></span>

<span data-ttu-id="d09b8-220">Cree una clase base que contiene los servicios usados en los contactos de las páginas de Razor.</span><span class="sxs-lookup"><span data-stu-id="d09b8-220">Create a base class that contains the services used in the contacts Razor Pages.</span></span> <span data-ttu-id="d09b8-221">La clase base coloca ese código de inicialización en una ubicación:</span><span class="sxs-lookup"><span data-stu-id="d09b8-221">The base class puts that initialization code in one location:</span></span>

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/DI_BasePageModel.cs)]

<span data-ttu-id="d09b8-222">El código anterior:</span><span class="sxs-lookup"><span data-stu-id="d09b8-222">The preceding code:</span></span>

* <span data-ttu-id="d09b8-223">Agrega el `IAuthorizationService` servicio para tener acceso a los controladores de autorización.</span><span class="sxs-lookup"><span data-stu-id="d09b8-223">Adds the `IAuthorizationService` service to access to the authorization handlers.</span></span>
* <span data-ttu-id="d09b8-224">Agrega la identidad `UserManager` servicio.</span><span class="sxs-lookup"><span data-stu-id="d09b8-224">Adds the Identity `UserManager` service.</span></span>
* <span data-ttu-id="d09b8-225">Agregue la `ApplicationDbContext`.</span><span class="sxs-lookup"><span data-stu-id="d09b8-225">Add the `ApplicationDbContext`.</span></span>

### <a name="update-the-createmodel"></a><span data-ttu-id="d09b8-226">Actualizar el CreateModel</span><span class="sxs-lookup"><span data-stu-id="d09b8-226">Update the CreateModel</span></span>

<span data-ttu-id="d09b8-227">Actualizar el constructor de modelo de página de create para usar la `DI_BasePageModel` clase base:</span><span class="sxs-lookup"><span data-stu-id="d09b8-227">Update the create page model constructor to use the `DI_BasePageModel` base class:</span></span>

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

<span data-ttu-id="d09b8-228">Actualización de la `CreateModel.OnPostAsync` método:</span><span class="sxs-lookup"><span data-stu-id="d09b8-228">Update the `CreateModel.OnPostAsync` method to:</span></span>

* <span data-ttu-id="d09b8-229">Agregar el identificador de usuario para el `Contact` modelo.</span><span class="sxs-lookup"><span data-stu-id="d09b8-229">Add the user ID to the `Contact` model.</span></span>
* <span data-ttu-id="d09b8-230">Llamar al controlador de autorización para comprobar que el usuario tiene permiso para crear contactos.</span><span class="sxs-lookup"><span data-stu-id="d09b8-230">Call the authorization handler to verify the user has permission to create contacts.</span></span>

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a><span data-ttu-id="d09b8-231">Actualizar la IndexModel</span><span class="sxs-lookup"><span data-stu-id="d09b8-231">Update the IndexModel</span></span>

<span data-ttu-id="d09b8-232">Actualización de la `OnGetAsync` método por lo que sólo los contactos aprobados se muestran a los usuarios en general:</span><span class="sxs-lookup"><span data-stu-id="d09b8-232">Update the `OnGetAsync` method so only approved contacts are shown to general users:</span></span>

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a><span data-ttu-id="d09b8-233">Actualizar la EditModel</span><span class="sxs-lookup"><span data-stu-id="d09b8-233">Update the EditModel</span></span>

<span data-ttu-id="d09b8-234">Agregue un controlador de autorización para comprobar que el usuario es propietario del contacto.</span><span class="sxs-lookup"><span data-stu-id="d09b8-234">Add an authorization handler to verify the user owns the contact.</span></span> <span data-ttu-id="d09b8-235">Dado que se está validando la autorización de recursos, el `[Authorize]` atributo no es suficiente.</span><span class="sxs-lookup"><span data-stu-id="d09b8-235">Because resource authorization is being validated, the `[Authorize]` attribute is not enough.</span></span> <span data-ttu-id="d09b8-236">La aplicación no tiene acceso al recurso cuando se evalúan los atributos.</span><span class="sxs-lookup"><span data-stu-id="d09b8-236">The app doesn't have access to the resource when attributes are evaluated.</span></span> <span data-ttu-id="d09b8-237">Autorización basada en recursos debe ser imperativo.</span><span class="sxs-lookup"><span data-stu-id="d09b8-237">Resource-based authorization must be imperative.</span></span> <span data-ttu-id="d09b8-238">Las comprobaciones deben realizarse una vez que la aplicación tenga acceso al recurso, cargándolo en el modelo de páginas o cargándolo en su propio controlador.</span><span class="sxs-lookup"><span data-stu-id="d09b8-238">Checks must be performed once the app has access to the resource, either by loading it in the page model or by loading it within the handler itself.</span></span> <span data-ttu-id="d09b8-239">Con frecuencia tener acceso al recurso pasando la clave de recurso.</span><span class="sxs-lookup"><span data-stu-id="d09b8-239">You frequently access the resource by passing in the resource key.</span></span>

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a><span data-ttu-id="d09b8-240">Actualizar la DeleteModel</span><span class="sxs-lookup"><span data-stu-id="d09b8-240">Update the DeleteModel</span></span>

<span data-ttu-id="d09b8-241">Actualizar el modelo de páginas de delete para usar el controlador de autorización para comprobar que el usuario tiene permiso delete en el contacto.</span><span class="sxs-lookup"><span data-stu-id="d09b8-241">Update the delete page model to use the authorization handler to verify the user has delete permission on the contact.</span></span>

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a><span data-ttu-id="d09b8-242">Insertar el servicio de autorización en las vistas</span><span class="sxs-lookup"><span data-stu-id="d09b8-242">Inject the authorization service into the views</span></span>

<span data-ttu-id="d09b8-243">Actualmente, se muestra en la interfaz de usuario edita y elimina los vínculos de datos que no se puede modificar el usuario.</span><span class="sxs-lookup"><span data-stu-id="d09b8-243">Currently, the UI shows edit and delete links for data the user can't modify.</span></span> <span data-ttu-id="d09b8-244">La interfaz de usuario es fijo aplicando el controlador de autorización a las vistas.</span><span class="sxs-lookup"><span data-stu-id="d09b8-244">The UI is fixed by applying the authorization handler to the views.</span></span>

<span data-ttu-id="d09b8-245">Insertar el servicio de autorización en el *Views/_ViewImports.cshtml* para que esté disponible para todas las vistas de archivos:</span><span class="sxs-lookup"><span data-stu-id="d09b8-245">Inject the authorization service in the *Views/_ViewImports.cshtml* file so it's available to all views:</span></span>

[!code-cshtml[](secure-data/samples/final2/Pages/_ViewImports.cshtml?highlight=6-9)]

<span data-ttu-id="d09b8-246">El marcado anterior agrega varias `using` instrucciones.</span><span class="sxs-lookup"><span data-stu-id="d09b8-246">The preceding markup adds several `using` statements.</span></span>

<span data-ttu-id="d09b8-247">Actualización de la **editar** y **eliminar** vincula *Pages/Contacts/Index.cshtml* por lo que solo se procesan para los usuarios con los permisos adecuados:</span><span class="sxs-lookup"><span data-stu-id="d09b8-247">Update the **Edit** and **Delete** links in *Pages/Contacts/Index.cshtml* so they're only rendered for users with the appropriate permissions:</span></span>

[!code-cshtml[](secure-data/samples/final2/Pages/Contacts/Index.cshtml?highlight=34-36,64-999)]

> [!WARNING]
> <span data-ttu-id="d09b8-248">Ocultar los vínculos de los usuarios que no tienen permiso para cambiar los datos no proteger la aplicación.</span><span class="sxs-lookup"><span data-stu-id="d09b8-248">Hiding links from users that don't have permission to change data doesn't secure the app.</span></span> <span data-ttu-id="d09b8-249">Ocultar los vínculos hace que la aplicación más fácil de usar mostrando vínculos solo es válido.</span><span class="sxs-lookup"><span data-stu-id="d09b8-249">Hiding links makes the app more user-friendly by displaying only valid links.</span></span> <span data-ttu-id="d09b8-250">Los usuarios pueden hack las direcciones URL generadas para invocar Editar y eliminar operaciones en los datos que no poseen.</span><span class="sxs-lookup"><span data-stu-id="d09b8-250">Users can hack the generated URLs to invoke edit and delete operations on data they don't own.</span></span> <span data-ttu-id="d09b8-251">La página de Razor o el controlador debe exigir comprobaciones de acceso para proteger los datos.</span><span class="sxs-lookup"><span data-stu-id="d09b8-251">The Razor Page or controller must enforce access checks to secure the data.</span></span>

### <a name="update-details"></a><span data-ttu-id="d09b8-252">Detalles de la actualización</span><span class="sxs-lookup"><span data-stu-id="d09b8-252">Update Details</span></span>

<span data-ttu-id="d09b8-253">Actualice la vista de detalles para que los administradores pueden aprobar o rechazar contactos:</span><span class="sxs-lookup"><span data-stu-id="d09b8-253">Update the details view so managers can approve or reject contacts:</span></span>

[!code-cshtml[](secure-data/samples/final2/Pages/Contacts/Details.cshtml?range=48-999)]

<span data-ttu-id="d09b8-254">Actualice el modelo de páginas de detalles:</span><span class="sxs-lookup"><span data-stu-id="d09b8-254">Update the details page model:</span></span>

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="test-the-completed-app"></a><span data-ttu-id="d09b8-255">Probar la aplicación completada</span><span class="sxs-lookup"><span data-stu-id="d09b8-255">Test the completed app</span></span>

<span data-ttu-id="d09b8-256">Si está usando Visual Studio Code o pruebas en una plataforma local que no incluye un certificado de prueba para HTTPS:</span><span class="sxs-lookup"><span data-stu-id="d09b8-256">If you're using Visual Studio Code or testing on a local platform that doesn't include a test certificate for HTTPS:</span></span>

* <span data-ttu-id="d09b8-257">Establecer `"LocalTest:skipHTTPS": true` en el *appsettings. Developement.JSON* archivo para omitir el requisito de HTTPS.</span><span class="sxs-lookup"><span data-stu-id="d09b8-257">Set `"LocalTest:skipHTTPS": true` in the *appsettings.Developement.json* file to skip the HTTPS requirement.</span></span> <span data-ttu-id="d09b8-258">Skip HTTPS solamente en un equipo de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="d09b8-258">Skip HTTPS only on a development machine.</span></span>

<span data-ttu-id="d09b8-259">Si la aplicación tiene contactos:</span><span class="sxs-lookup"><span data-stu-id="d09b8-259">If the app has contacts:</span></span>

* <span data-ttu-id="d09b8-260">Eliminar todos los registros en la `Contact` tabla.</span><span class="sxs-lookup"><span data-stu-id="d09b8-260">Delete all the records in the `Contact` table.</span></span>
* <span data-ttu-id="d09b8-261">Reinicie la aplicación para inicializar la base de datos.</span><span class="sxs-lookup"><span data-stu-id="d09b8-261">Restart the app to seed the database.</span></span>

<span data-ttu-id="d09b8-262">Registrar un usuario para examinar los contactos.</span><span class="sxs-lookup"><span data-stu-id="d09b8-262">Register a user for browsing the contacts.</span></span>

<span data-ttu-id="d09b8-263">Una manera sencilla de probar la aplicación final consiste en iniciar tres distintos exploradores (o versiones de incógnito/InPrivate).</span><span class="sxs-lookup"><span data-stu-id="d09b8-263">An easy way to test the completed app is to launch three different browsers (or incognito/InPrivate versions).</span></span> <span data-ttu-id="d09b8-264">En un explorador, registrar un nuevo usuario (por ejemplo, `test@example.com`).</span><span class="sxs-lookup"><span data-stu-id="d09b8-264">In one browser, register a new user (for example, `test@example.com`).</span></span> <span data-ttu-id="d09b8-265">Inicie sesión con un usuario diferente en cada explorador.</span><span class="sxs-lookup"><span data-stu-id="d09b8-265">Sign in to each browser with a different user.</span></span> <span data-ttu-id="d09b8-266">Compruebe las operaciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="d09b8-266">Verify the following operations:</span></span>

* <span data-ttu-id="d09b8-267">Los usuarios registrados pueden ver todos los datos de contacto aprobados.</span><span class="sxs-lookup"><span data-stu-id="d09b8-267">Registered users can view all the approved contact data.</span></span>
* <span data-ttu-id="d09b8-268">Los usuarios registrados pueden editar o eliminar sus propios datos.</span><span class="sxs-lookup"><span data-stu-id="d09b8-268">Registered users can edit/delete their own data.</span></span>
* <span data-ttu-id="d09b8-269">Los administradores pueden aprobar o rechazar datos de contacto.</span><span class="sxs-lookup"><span data-stu-id="d09b8-269">Managers can approve or reject contact data.</span></span> <span data-ttu-id="d09b8-270">El `Details` vista muestra **aprobar** y **rechazar** botones.</span><span class="sxs-lookup"><span data-stu-id="d09b8-270">The `Details` view shows **Approve** and **Reject** buttons.</span></span>
* <span data-ttu-id="d09b8-271">Los administradores pueden aprobar o rechazar y editar o eliminar los datos.</span><span class="sxs-lookup"><span data-stu-id="d09b8-271">Administrators can approve/reject and edit/delete any data.</span></span>

| <span data-ttu-id="d09b8-272">Usuario</span><span class="sxs-lookup"><span data-stu-id="d09b8-272">User</span></span>| <span data-ttu-id="d09b8-273">Opciones</span><span class="sxs-lookup"><span data-stu-id="d09b8-273">Options</span></span> |
| ------------ | ---------|
| test@example.com | <span data-ttu-id="d09b8-274">Editar puede/eliminar datos propios</span><span class="sxs-lookup"><span data-stu-id="d09b8-274">Can edit/delete own data</span></span> |
| manager@contoso.com | <span data-ttu-id="d09b8-275">Pueden aprobar o rechazar y editar o eliminar datos de propietario</span><span class="sxs-lookup"><span data-stu-id="d09b8-275">Can approve/reject and edit/delete own data</span></span> |
| admin@contoso.com | <span data-ttu-id="d09b8-276">Puede editar o eliminar y aprobar o rechazar todos los datos</span><span class="sxs-lookup"><span data-stu-id="d09b8-276">Can edit/delete and approve/reject all data</span></span>|

<span data-ttu-id="d09b8-277">Crear un contacto en el explorador del administrador.</span><span class="sxs-lookup"><span data-stu-id="d09b8-277">Create a contact in the administrator's browser.</span></span> <span data-ttu-id="d09b8-278">Copie la dirección URL para eliminar y editar en el contacto del administrador.</span><span class="sxs-lookup"><span data-stu-id="d09b8-278">Copy the URL for delete and edit from the administrator contact.</span></span> <span data-ttu-id="d09b8-279">Pegue estos vínculos en el explorador del usuario de prueba para comprobar que el usuario de prueba no puede realizar estas operaciones.</span><span class="sxs-lookup"><span data-stu-id="d09b8-279">Paste these links into the test user's browser to verify the test user can't perform these operations.</span></span>

## <a name="create-the-starter-app"></a><span data-ttu-id="d09b8-280">Crear la aplicación de inicio</span><span class="sxs-lookup"><span data-stu-id="d09b8-280">Create the starter app</span></span>

* <span data-ttu-id="d09b8-281">Crear una aplicación de páginas de Razor denominada "ContactManager"</span><span class="sxs-lookup"><span data-stu-id="d09b8-281">Create a Razor Pages app named "ContactManager"</span></span>

  * <span data-ttu-id="d09b8-282">Crear la aplicación con **cuentas de usuario individuales**.</span><span class="sxs-lookup"><span data-stu-id="d09b8-282">Create the app with **Individual User Accounts**.</span></span>
  * <span data-ttu-id="d09b8-283">Asígnele el nombre "ContactManager" para el espacio de nombres coincide con el espacio de nombres utilizado en el ejemplo.</span><span class="sxs-lookup"><span data-stu-id="d09b8-283">Name it "ContactManager" so your namespace matches the namespace used in the sample.</span></span>

::: moniker range=">= aspnetcore-2.1"

  ```console
  dotnet new webapp -o ContactManager -au Individual -uld
  ```

  [!INCLUDE[](~/includes/webapp-alias-notice.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

  ```console
  dotnet new razor -o ContactManager -au Individual -uld
  ```

::: moniker-end

  * <span data-ttu-id="d09b8-285">`-uld` Especifica LocalDB en lugar de SQLite</span><span class="sxs-lookup"><span data-stu-id="d09b8-285">`-uld` specifies LocalDB instead of SQLite</span></span>

* <span data-ttu-id="d09b8-286">Agregue las siguientes `Contact` modelo:</span><span class="sxs-lookup"><span data-stu-id="d09b8-286">Add the following `Contact` model:</span></span>

  [!code-csharp[](secure-data/samples/starter2/Models/Contact.cs?name=snippet1)]

* <span data-ttu-id="d09b8-287">Scaffold el `Contact` modelo:</span><span class="sxs-lookup"><span data-stu-id="d09b8-287">Scaffold the `Contact` model:</span></span>

```console
dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
```

* <span data-ttu-id="d09b8-288">Actualización de la **ContactManager** fijar en el *Pages/_Layout.cshtml* archivo:</span><span class="sxs-lookup"><span data-stu-id="d09b8-288">Update the **ContactManager** anchor in the *Pages/_Layout.cshtml* file:</span></span>

```cshtml
<a asp-page="/Contacts/Index" class="navbar-brand">ContactManager</a>
```

* <span data-ttu-id="d09b8-289">Aplicar la técnica scaffolding la migración inicial y actualizar la base de datos:</span><span class="sxs-lookup"><span data-stu-id="d09b8-289">Scaffold the initial migration and update the database:</span></span>

```console
dotnet ef migrations add initial
dotnet ef database update
```

* <span data-ttu-id="d09b8-290">Probar la aplicación mediante la creación, edición y eliminación de un contacto</span><span class="sxs-lookup"><span data-stu-id="d09b8-290">Test the app by creating, editing, and deleting a contact</span></span>

### <a name="seed-the-database"></a><span data-ttu-id="d09b8-291">Inicializar la base de datos</span><span class="sxs-lookup"><span data-stu-id="d09b8-291">Seed the database</span></span>

<span data-ttu-id="d09b8-292">Agregar el `SeedData` clase a la *datos* carpeta.</span><span class="sxs-lookup"><span data-stu-id="d09b8-292">Add the `SeedData` class to the *Data* folder.</span></span> <span data-ttu-id="d09b8-293">Si ha descargado el ejemplo, puede copiar la *SeedData.cs* del archivo a la *datos* carpeta del proyecto de inicio.</span><span class="sxs-lookup"><span data-stu-id="d09b8-293">If you've downloaded the sample, you can copy the *SeedData.cs* file to the *Data* folder of the starter project.</span></span>

<span data-ttu-id="d09b8-294">Llame a `SeedData.Initialize` de `Main`:</span><span class="sxs-lookup"><span data-stu-id="d09b8-294">Call `SeedData.Initialize` from `Main`:</span></span>

[!code-csharp[](secure-data/samples/starter2/Program.cs?name=snippet)]

<span data-ttu-id="d09b8-295">Compruebe que la aplicación había propagado la base de datos.</span><span class="sxs-lookup"><span data-stu-id="d09b8-295">Test that the app seeded the database.</span></span> <span data-ttu-id="d09b8-296">Si no hay ninguna fila en la base de datos de contacto, no se ejecuta el método de inicialización.</span><span class="sxs-lookup"><span data-stu-id="d09b8-296">If there are any rows in the contact DB, the seed method doesn't run.</span></span>

<a name="secure-data-add-resources-label"></a>

### <a name="additional-resources"></a><span data-ttu-id="d09b8-297">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="d09b8-297">Additional resources</span></span>

* <span data-ttu-id="d09b8-298">[Laboratorio de autorización de ASP.NET Core](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span><span class="sxs-lookup"><span data-stu-id="d09b8-298">[ASP.NET Core Authorization Lab](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span></span> <span data-ttu-id="d09b8-299">Este laboratorio se explica con más detalle en las características de seguridad introducidas en este tutorial.</span><span class="sxs-lookup"><span data-stu-id="d09b8-299">This lab goes into more detail on the security features introduced in this tutorial.</span></span>
* [<span data-ttu-id="d09b8-300">Autorización de ASP.NET Core: Simple, rol, basada en notificaciones y personalizado</span><span class="sxs-lookup"><span data-stu-id="d09b8-300">Authorization in ASP.NET Core: Simple, role, claims-based, and custom</span></span>](xref:security/authorization/index)
* [<span data-ttu-id="d09b8-301">Autorización personalizada basada en directivas</span><span class="sxs-lookup"><span data-stu-id="d09b8-301">Custom policy-based authorization</span></span>](xref:security/authorization/policies)
