---
title: "Crear una aplicación de ASP.NET Core con datos de usuario protegidos por autorización"
author: rick-anderson
description: "Obtenga información acerca de cómo crear una aplicación de páginas de Razor con datos de usuario protegidos mediante autorización. Incluye HTTPS, la autenticación, la seguridad, ASP.NET Core Identity."
manager: wpickett
ms.author: riande
ms.date: 01/24/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/secure-data
ms.openlocfilehash: e186adef2e72f852543a92ddce0e82be2a3bcd12
ms.sourcegitcommit: 809ee4baf8bf7b4cae9e366ecae29de1037d2bbb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/15/2018
---
# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a><span data-ttu-id="b67d2-104">Crear una aplicación de ASP.NET Core con datos de usuario protegidos por autorización</span><span class="sxs-lookup"><span data-stu-id="b67d2-104">Create an ASP.NET Core app with user data protected by authorization</span></span>

<span data-ttu-id="b67d2-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT) y [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="b67d2-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="b67d2-106">Este tutorial muestra cómo crear una aplicación web de ASP.NET Core con datos de usuario protegidos mediante autorización.</span><span class="sxs-lookup"><span data-stu-id="b67d2-106">This tutorial shows how to create an ASP.NET Core web app with user data protected by authorization.</span></span> <span data-ttu-id="b67d2-107">Muestra una lista de contactos que autentica los usuarios (registrados) ha creado.</span><span class="sxs-lookup"><span data-stu-id="b67d2-107">It displays a list of contacts that authenticated (registered) users have created.</span></span> <span data-ttu-id="b67d2-108">Hay tres grupos de seguridad:</span><span class="sxs-lookup"><span data-stu-id="b67d2-108">There are three security groups:</span></span>

* <span data-ttu-id="b67d2-109">**Usuarios registrados** puede ver todos los datos aprobados y editar puede/eliminar sus propios datos.</span><span class="sxs-lookup"><span data-stu-id="b67d2-109">**Registered users** can view all the approved data and can edit/delete their own data.</span></span>
* <span data-ttu-id="b67d2-110">**Administradores de** puede aprobar o rechazar los datos de contacto.</span><span class="sxs-lookup"><span data-stu-id="b67d2-110">**Managers** can approve or reject contact data.</span></span> <span data-ttu-id="b67d2-111">Sólo los contactos aprobados son visibles para los usuarios.</span><span class="sxs-lookup"><span data-stu-id="b67d2-111">Only approved contacts are visible to users.</span></span>
* <span data-ttu-id="b67d2-112">**Los administradores** puede aprobar o rechazar y editar o eliminar los datos.</span><span class="sxs-lookup"><span data-stu-id="b67d2-112">**Administrators** can approve/reject and edit/delete any data.</span></span>

<span data-ttu-id="b67d2-113">En la siguiente imagen, usuario Rick (`rick@example.com`) ha iniciado sesión.</span><span class="sxs-lookup"><span data-stu-id="b67d2-113">In the following image, user Rick (`rick@example.com`) is signed in.</span></span> <span data-ttu-id="b67d2-114">Rick solo puede ver los contactos aprobados y **editar**/**eliminar**/**crear nuevo** vínculos de sus contactos.</span><span class="sxs-lookup"><span data-stu-id="b67d2-114">Rick can only view approved contacts and **Edit**/**Delete**/**Create New** links for his contacts.</span></span> <span data-ttu-id="b67d2-115">El último registro, creado por Rick, muestra **editar** y **eliminar** vínculos.</span><span class="sxs-lookup"><span data-stu-id="b67d2-115">Only the last record, created by Rick, displays **Edit** and **Delete** links.</span></span> <span data-ttu-id="b67d2-116">Otros usuarios no verán el último registro hasta que un administrador o un administrador cambia el estado a "Aprobado".</span><span class="sxs-lookup"><span data-stu-id="b67d2-116">Other users won't see the last record until a manager or administrator changes the status to "Approved".</span></span>

![imagen describe anterior](secure-data/_static/rick.png)

<span data-ttu-id="b67d2-118">En la siguiente imagen, `manager@contoso.com` está firmado en y en el rol de administrador:</span><span class="sxs-lookup"><span data-stu-id="b67d2-118">In the following image, `manager@contoso.com` is signed in and in the managers role:</span></span>

![imagen describe anterior](secure-data/_static/manager1.png)

<span data-ttu-id="b67d2-120">La siguiente imagen muestra a los administradores de la vista de detalles de un contacto:</span><span class="sxs-lookup"><span data-stu-id="b67d2-120">The following image shows the managers details view of a contact:</span></span>

![imagen describe anterior](secure-data/_static/manager.png)

<span data-ttu-id="b67d2-122">El **aprobar** y **rechazar** solo se muestran los botones para los administradores y administradores.</span><span class="sxs-lookup"><span data-stu-id="b67d2-122">The **Approve** and **Reject** buttons are only displayed for managers and administrators.</span></span>

<span data-ttu-id="b67d2-123">En la siguiente imagen, `admin@contoso.com` está firmado en y en la función Administradores:</span><span class="sxs-lookup"><span data-stu-id="b67d2-123">In the following image, `admin@contoso.com` is signed in and in the administrators role:</span></span>

![imagen describe anterior](secure-data/_static/admin.png)

<span data-ttu-id="b67d2-125">El administrador tiene todos los privilegios.</span><span class="sxs-lookup"><span data-stu-id="b67d2-125">The administrator has all privileges.</span></span> <span data-ttu-id="b67d2-126">Puede leer, editar o eliminar cualquier contacto y cambiar el estado de contactos.</span><span class="sxs-lookup"><span data-stu-id="b67d2-126">She can read/edit/delete any contact and change the status of contacts.</span></span>

<span data-ttu-id="b67d2-127">La aplicación se creó con [scaffolding](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller) siguiente `Contact` modelo:</span><span class="sxs-lookup"><span data-stu-id="b67d2-127">The app was created by [scaffolding](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller) the following `Contact` model:</span></span>

[!code-csharp[Main](secure-data/samples/starter2/Models/Contact.cs?name=snippet1)]

<span data-ttu-id="b67d2-128">El ejemplo contiene los siguientes controladores de autorización:</span><span class="sxs-lookup"><span data-stu-id="b67d2-128">The sample contains the following authorization handlers:</span></span>

* <span data-ttu-id="b67d2-129">`ContactIsOwnerAuthorizationHandler`: Se asegura de que un usuario solo puede editar sus datos.</span><span class="sxs-lookup"><span data-stu-id="b67d2-129">`ContactIsOwnerAuthorizationHandler`: Ensures that a user can only edit their data.</span></span>
* <span data-ttu-id="b67d2-130">`ContactManagerAuthorizationHandler`: Permite a los administradores aprobar o rechazar los contactos.</span><span class="sxs-lookup"><span data-stu-id="b67d2-130">`ContactManagerAuthorizationHandler`: Allows managers to approve or reject contacts.</span></span>
* <span data-ttu-id="b67d2-131">`ContactAdministratorsAuthorizationHandler`: Permite a los administradores para aprobar o rechazar los contactos y editar o eliminar contactos.</span><span class="sxs-lookup"><span data-stu-id="b67d2-131">`ContactAdministratorsAuthorizationHandler`: Allows administrators to approve or reject contacts and to edit/delete contacts.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b67d2-132">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="b67d2-132">Prerequisites</span></span>

<span data-ttu-id="b67d2-133">Este tutorial se avanza.</span><span class="sxs-lookup"><span data-stu-id="b67d2-133">This tutorial is advanced.</span></span> <span data-ttu-id="b67d2-134">Debe estar familiarizado con:</span><span class="sxs-lookup"><span data-stu-id="b67d2-134">You should be familiar with:</span></span>

* [<span data-ttu-id="b67d2-135">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b67d2-135">ASP.NET Core</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="b67d2-136">Autenticación</span><span class="sxs-lookup"><span data-stu-id="b67d2-136">Authentication</span></span>](xref:security/authentication/index)
* [<span data-ttu-id="b67d2-137">Confirmación de cuentas y recuperación de contraseñas</span><span class="sxs-lookup"><span data-stu-id="b67d2-137">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="b67d2-138">Autorización</span><span class="sxs-lookup"><span data-stu-id="b67d2-138">Authorization</span></span>](xref:security/authorization/index)
* [<span data-ttu-id="b67d2-139">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="b67d2-139">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

<span data-ttu-id="b67d2-140">Vea [este archivo PDF](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) para la versión principal de ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="b67d2-140">See [this PDF file](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core MVC version.</span></span> <span data-ttu-id="b67d2-141">La versión 1.1 de ASP.NET Core de este tutorial está en [esto](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data) carpeta.</span><span class="sxs-lookup"><span data-stu-id="b67d2-141">The ASP.NET Core 1.1 version of this tutorial is in [this](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data) folder.</span></span> <span data-ttu-id="b67d2-142">La 1.1 incluye el ejemplo de ASP.NET Core el [ejemplos](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).</span><span class="sxs-lookup"><span data-stu-id="b67d2-142">The 1.1 ASP.NET Core sample is in the [samples](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).</span></span>

## <a name="the-starter-and-completed-app"></a><span data-ttu-id="b67d2-143">El inicio y la aplicación completada</span><span class="sxs-lookup"><span data-stu-id="b67d2-143">The starter and completed app</span></span>

<span data-ttu-id="b67d2-144">[Descargar](xref:tutorials/index#how-to-download-a-sample) el [completado](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2) aplicación.</span><span class="sxs-lookup"><span data-stu-id="b67d2-144">[Download](xref:tutorials/index#how-to-download-a-sample) the [completed](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2) app.</span></span> <span data-ttu-id="b67d2-145">[Prueba](#test-the-completed-app) la aplicación completada por lo que se familiarice con sus características de seguridad.</span><span class="sxs-lookup"><span data-stu-id="b67d2-145">[Test](#test-the-completed-app) the completed app so you become familiar with its security features.</span></span>

### <a name="the-starter-app"></a><span data-ttu-id="b67d2-146">La aplicación de inicio</span><span class="sxs-lookup"><span data-stu-id="b67d2-146">The starter app</span></span>

<span data-ttu-id="b67d2-147">[Descargar](xref:tutorials/index#how-to-download-a-sample) el [starter](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2) aplicación.</span><span class="sxs-lookup"><span data-stu-id="b67d2-147">[Download](xref:tutorials/index#how-to-download-a-sample) the [starter](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2) app.</span></span>

<span data-ttu-id="b67d2-148">Ejecutar la aplicación, pulse el **ContactManager** vincular y compruebe que puede crear, editar y eliminar un contacto.</span><span class="sxs-lookup"><span data-stu-id="b67d2-148">Run the app, tap the **ContactManager** link, and verify you can create, edit, and delete a contact.</span></span>

## <a name="secure-user-data"></a><span data-ttu-id="b67d2-149">Proteger los datos de usuario</span><span class="sxs-lookup"><span data-stu-id="b67d2-149">Secure user data</span></span>

<span data-ttu-id="b67d2-150">Las siguientes secciones contienen todos los pasos principales para crear la aplicación de datos de usuario seguras.</span><span class="sxs-lookup"><span data-stu-id="b67d2-150">The following sections have all the major steps to create the secure user data app.</span></span> <span data-ttu-id="b67d2-151">Le resultará útil para hacer referencia al proyecto completado.</span><span class="sxs-lookup"><span data-stu-id="b67d2-151">You may find it helpful to refer to the completed project.</span></span>

### <a name="tie-the-contact-data-to-the-user"></a><span data-ttu-id="b67d2-152">Asociar los datos de contacto para el usuario</span><span class="sxs-lookup"><span data-stu-id="b67d2-152">Tie the contact data to the user</span></span>

<span data-ttu-id="b67d2-153">Usar ASP.NET [identidad](xref:security/authentication/identity) Id. de usuario para garantizar que los usuarios puede editar sus datos, pero no otros datos de los usuarios.</span><span class="sxs-lookup"><span data-stu-id="b67d2-153">Use the ASP.NET [Identity](xref:security/authentication/identity) user ID to ensure users can edit their data, but not other users data.</span></span> <span data-ttu-id="b67d2-154">Agregar `OwnerID` y `ContactStatus` a la `Contact` modelo:</span><span class="sxs-lookup"><span data-stu-id="b67d2-154">Add `OwnerID` and `ContactStatus` to the `Contact` model:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Models/Contact.cs?name=snippet1&highlight=5-6,16-999)]

<span data-ttu-id="b67d2-155">`OwnerID` es el identificador del usuario desde el `AspNetUser` tabla el [identidad](xref:security/authentication/identity) base de datos.</span><span class="sxs-lookup"><span data-stu-id="b67d2-155">`OwnerID` is the user's ID from the `AspNetUser` table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="b67d2-156">El `Status` campo determina si un contacto es visible para los usuarios en general.</span><span class="sxs-lookup"><span data-stu-id="b67d2-156">The `Status` field determines if a contact is viewable by general users.</span></span>

<span data-ttu-id="b67d2-157">Crear una nueva migración y actualizar la base de datos:</span><span class="sxs-lookup"><span data-stu-id="b67d2-157">Create a new migration and update the database:</span></span>

```console
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="require-https-and-authenticated-users"></a><span data-ttu-id="b67d2-158">Requerir HTTPS y los usuarios autenticados</span><span class="sxs-lookup"><span data-stu-id="b67d2-158">Require HTTPS and authenticated users</span></span>

<span data-ttu-id="b67d2-159">Agregar [IHostingEnvironment](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) a `Startup`:</span><span class="sxs-lookup"><span data-stu-id="b67d2-159">Add [IHostingEnvironment](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) to `Startup`:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Startup.cs?name=snippet_env)]

<span data-ttu-id="b67d2-160">En el `ConfigureServices` método de la *Startup.cs* , agregue el [RequireHttpsAttribute](/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) filtro de autorización:</span><span class="sxs-lookup"><span data-stu-id="b67d2-160">In the `ConfigureServices` method of the *Startup.cs* file, add the [RequireHttpsAttribute](/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) authorization filter:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Startup.cs?name=snippet_SSL&highlight=10-999)]

<span data-ttu-id="b67d2-161">Si está utilizando Visual Studio, habilitar HTTPS.</span><span class="sxs-lookup"><span data-stu-id="b67d2-161">If you're using Visual Studio, enable HTTPS.</span></span>

<span data-ttu-id="b67d2-162">Para redirigir las solicitudes HTTP a HTTPS, consulte [Middleware de reescritura de dirección URL](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="b67d2-162">To redirect HTTP requests to HTTPS, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span> <span data-ttu-id="b67d2-163">Si está usando Visual Studio Code o pruebas en una plataforma local que no incluye un certificado de prueba para HTTPS:</span><span class="sxs-lookup"><span data-stu-id="b67d2-163">If you're using Visual Studio Code or testing on a local platform that doesn't include a test certificate for HTTPS:</span></span>

  <span data-ttu-id="b67d2-164">Establecer `"LocalTest:skipSSL": true` en el *appsettings. Developement.JSON* archivo.</span><span class="sxs-lookup"><span data-stu-id="b67d2-164">Set `"LocalTest:skipSSL": true` in the *appsettings.Developement.json* file.</span></span>

### <a name="require-authenticated-users"></a><span data-ttu-id="b67d2-165">Requerir a los usuarios autenticados</span><span class="sxs-lookup"><span data-stu-id="b67d2-165">Require authenticated users</span></span>

<span data-ttu-id="b67d2-166">Establecer la directiva de autenticación predeterminado para exigir que los usuarios se autentiquen.</span><span class="sxs-lookup"><span data-stu-id="b67d2-166">Set the default authentication policy to require users to be authenticated.</span></span> <span data-ttu-id="b67d2-167">Puede rechazar la autenticación en el nivel de método página Razor, controlador o acción con el `[AllowAnonymous]` atributo.</span><span class="sxs-lookup"><span data-stu-id="b67d2-167">You can opt out of authentication at the Razor Page, controller, or action method level with the `[AllowAnonymous]` attribute.</span></span> <span data-ttu-id="b67d2-168">Establecer la directiva de autenticación predeterminado para exigir que los usuarios se autentiquen protege recién agregado las páginas Razor y controladores.</span><span class="sxs-lookup"><span data-stu-id="b67d2-168">Setting the default authentication policy to require users to be authenticated protects newly added Razor Pages and controllers.</span></span> <span data-ttu-id="b67d2-169">Existencia de autenticación que requiere de forma predeterminada es más segura que confiar en los nuevos controladores y las páginas de Razor para incluir la `[Authorize]` atributo.</span><span class="sxs-lookup"><span data-stu-id="b67d2-169">Having authentication required by default is safer than relying on new controllers and Razor Pages to include the `[Authorize]` attribute.</span></span> 

<span data-ttu-id="b67d2-170">Con el requisito de todos los usuarios autenticados, el [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder?view=aspnetcore-2.0#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) y [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage?view=aspnetcore-2.0) llamadas no son necesarias.</span><span class="sxs-lookup"><span data-stu-id="b67d2-170">With the requirement of all users authenticated, the [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder?view=aspnetcore-2.0#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) and [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage?view=aspnetcore-2.0) calls are not required.</span></span>

<span data-ttu-id="b67d2-171">Actualización `ConfigureServices` con los cambios siguientes:</span><span class="sxs-lookup"><span data-stu-id="b67d2-171">Update `ConfigureServices` with the following changes:</span></span>

* <span data-ttu-id="b67d2-172">Convertir en comentario `AuthorizeFolder` y `AuthorizePage`.</span><span class="sxs-lookup"><span data-stu-id="b67d2-172">Comment out `AuthorizeFolder` and `AuthorizePage`.</span></span>
* <span data-ttu-id="b67d2-173">Establecer la directiva de autenticación predeterminado para exigir que los usuarios se autentiquen.</span><span class="sxs-lookup"><span data-stu-id="b67d2-173">Set the default authentication policy to require users to be authenticated.</span></span>

[!code-csharp[Main](secure-data/samples/final2/Startup.cs?name=snippet_defaultPolicy&highlight=23-27,31-999)]

<span data-ttu-id="b67d2-174">Agregar [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) al índice, las páginas sobre y, a continuación, póngase en contacto con por lo que los usuarios anónimos pueden recibir información sobre el sitio antes de que registrar.</span><span class="sxs-lookup"><span data-stu-id="b67d2-174">Add [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) to the Index, About, and Contact pages so anonymous users can get information about the site before they register.</span></span> 

[!code-csharp[Main](secure-data/samples/final2/Pages/Index.cshtml.cs?name=snippet&highlight=2)]

<span data-ttu-id="b67d2-175">Agregar `[AllowAnonymous]` a la [LoginModel y RegisterModel](https://github.com/aspnet/templating/issues/238).</span><span class="sxs-lookup"><span data-stu-id="b67d2-175">Add `[AllowAnonymous]` to the [LoginModel and RegisterModel](https://github.com/aspnet/templating/issues/238).</span></span>

### <a name="configure-the-test-account"></a><span data-ttu-id="b67d2-176">Configurar la cuenta de prueba</span><span class="sxs-lookup"><span data-stu-id="b67d2-176">Configure the test account</span></span>

<span data-ttu-id="b67d2-177">La `SeedData` clase crea dos cuentas: administrador y el administrador.</span><span class="sxs-lookup"><span data-stu-id="b67d2-177">The `SeedData` class creates two accounts: administrator and manager.</span></span> <span data-ttu-id="b67d2-178">Use la [herramienta secreto administrador](xref:security/app-secrets) para establecer una contraseña para estas cuentas.</span><span class="sxs-lookup"><span data-stu-id="b67d2-178">Use the [Secret Manager tool](xref:security/app-secrets) to set a password for these accounts.</span></span> <span data-ttu-id="b67d2-179">Establecer la contraseña desde el directorio del proyecto (el directorio que contiene *Program.cs*):</span><span class="sxs-lookup"><span data-stu-id="b67d2-179">Set the password from the project directory (the directory containing *Program.cs*):</span></span>

```console
dotnet user-secrets set SeedUserPW <PW>
```

<span data-ttu-id="b67d2-180">Actualización `Main` usar la contraseña de prueba:</span><span class="sxs-lookup"><span data-stu-id="b67d2-180">Update `Main` to use the test password:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a><span data-ttu-id="b67d2-181">Crear las cuentas de prueba y actualizar los contactos</span><span class="sxs-lookup"><span data-stu-id="b67d2-181">Create the test accounts and update the contacts</span></span>

<span data-ttu-id="b67d2-182">Actualización de la `Initialize` método en la `SeedData` clase para crear las cuentas de prueba:</span><span class="sxs-lookup"><span data-stu-id="b67d2-182">Update the `Initialize` method in the `SeedData` class to create the test accounts:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Data/SeedData.cs?name=snippet_Initialize)]

<span data-ttu-id="b67d2-183">Agregue el identificador de usuario de administrador y `ContactStatus` a los contactos.</span><span class="sxs-lookup"><span data-stu-id="b67d2-183">Add the administrator user ID and `ContactStatus` to the contacts.</span></span> <span data-ttu-id="b67d2-184">Realice uno de los contactos "Enviado" y un "rechazada".</span><span class="sxs-lookup"><span data-stu-id="b67d2-184">Make one of the contacts "Submitted" and one "Rejected".</span></span> <span data-ttu-id="b67d2-185">Agregue el Id. de usuario y el estado para todos los contactos.</span><span class="sxs-lookup"><span data-stu-id="b67d2-185">Add the user ID and status to all the contacts.</span></span> <span data-ttu-id="b67d2-186">Póngase en contacto un solo con se muestra:</span><span class="sxs-lookup"><span data-stu-id="b67d2-186">Only one contact is shown:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a><span data-ttu-id="b67d2-187">Crear propietario, el administrador y los controladores de autorización de administrador</span><span class="sxs-lookup"><span data-stu-id="b67d2-187">Create owner, manager, and administrator authorization handlers</span></span>

<span data-ttu-id="b67d2-188">Crear un `ContactIsOwnerAuthorizationHandler` clase en el *autorización* carpeta.</span><span class="sxs-lookup"><span data-stu-id="b67d2-188">Create a `ContactIsOwnerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="b67d2-189">El `ContactIsOwnerAuthorizationHandler` comprueba que el usuario que actúa en un recurso posee el recurso.</span><span class="sxs-lookup"><span data-stu-id="b67d2-189">The `ContactIsOwnerAuthorizationHandler` verifies that the user acting on a resource owns the resource.</span></span>

[!code-csharp[Main](secure-data/samples/final2/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

<span data-ttu-id="b67d2-190">El `ContactIsOwnerAuthorizationHandler` llamadas [contexto. Correctamente](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) si el usuario autenticado actual es el propietario del contacto.</span><span class="sxs-lookup"><span data-stu-id="b67d2-190">The `ContactIsOwnerAuthorizationHandler` calls [context.Succeed](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) if the current authenticated user is the contact owner.</span></span> <span data-ttu-id="b67d2-191">Controladores de autorización general:</span><span class="sxs-lookup"><span data-stu-id="b67d2-191">Authorization handlers generally:</span></span>

* <span data-ttu-id="b67d2-192">Devolver `context.Succeed` cuando se cumplen los requisitos.</span><span class="sxs-lookup"><span data-stu-id="b67d2-192">Return `context.Succeed` when the requirements are met.</span></span>
* <span data-ttu-id="b67d2-193">Devolver `Task.CompletedTask` cuando no se cumplen los requisitos.</span><span class="sxs-lookup"><span data-stu-id="b67d2-193">Return `Task.CompletedTask` when requirements aren't met.</span></span> <span data-ttu-id="b67d2-194">`Task.CompletedTask` no correcto o error&mdash;permite que otros controladores de autorización para que se ejecute.</span><span class="sxs-lookup"><span data-stu-id="b67d2-194">`Task.CompletedTask` is neither success or failure&mdash;it allows other authorization handlers to run.</span></span>

<span data-ttu-id="b67d2-195">Si necesita explícitamente un error, devolver [contexto. Un error](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span><span class="sxs-lookup"><span data-stu-id="b67d2-195">If you need to explicitly fail, return [context.Fail](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span></span>

<span data-ttu-id="b67d2-196">La aplicación permite a los propietarios de contacto para editar, eliminar o crear sus propios datos.</span><span class="sxs-lookup"><span data-stu-id="b67d2-196">The app allows contact owners to edit/delete/create their own data.</span></span> <span data-ttu-id="b67d2-197">`ContactIsOwnerAuthorizationHandler` no tiene que comprobar la operación pasada en el parámetro de requisito.</span><span class="sxs-lookup"><span data-stu-id="b67d2-197">`ContactIsOwnerAuthorizationHandler` doesn't need to check the operation passed in the requirement parameter.</span></span>

### <a name="create-a-manager-authorization-handler"></a><span data-ttu-id="b67d2-198">Crear un controlador del Administrador de autorización</span><span class="sxs-lookup"><span data-stu-id="b67d2-198">Create a manager authorization handler</span></span>

<span data-ttu-id="b67d2-199">Crear un `ContactManagerAuthorizationHandler` clase en el *autorización* carpeta.</span><span class="sxs-lookup"><span data-stu-id="b67d2-199">Create a `ContactManagerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="b67d2-200">El `ContactManagerAuthorizationHandler` comprueba que el usuario que actúa en el recurso es un administrador.</span><span class="sxs-lookup"><span data-stu-id="b67d2-200">The `ContactManagerAuthorizationHandler` verifies the user acting on the resource is a manager.</span></span> <span data-ttu-id="b67d2-201">Solo los administradores pueden aprobar o rechazar los cambios de contenido (nuevos o modificados).</span><span class="sxs-lookup"><span data-stu-id="b67d2-201">Only managers can approve or reject content changes (new or changed).</span></span>

[!code-csharp[Main](secure-data/samples/final2/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a><span data-ttu-id="b67d2-202">Crear un controlador de autorización de administrador</span><span class="sxs-lookup"><span data-stu-id="b67d2-202">Create an administrator authorization handler</span></span>

<span data-ttu-id="b67d2-203">Crear un `ContactAdministratorsAuthorizationHandler` clase en el *autorización* carpeta.</span><span class="sxs-lookup"><span data-stu-id="b67d2-203">Create a `ContactAdministratorsAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="b67d2-204">El `ContactAdministratorsAuthorizationHandler` comprueba que el usuario que actúa en el recurso es un administrador.</span><span class="sxs-lookup"><span data-stu-id="b67d2-204">The `ContactAdministratorsAuthorizationHandler` verifies the user acting on the resource is an administrator.</span></span> <span data-ttu-id="b67d2-205">Administrador puede hacer que todas las operaciones.</span><span class="sxs-lookup"><span data-stu-id="b67d2-205">Administrator can do all operations.</span></span>

[!code-csharp[Main](secure-data/samples/final2/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a><span data-ttu-id="b67d2-206">Registrar los controladores de autorización</span><span class="sxs-lookup"><span data-stu-id="b67d2-206">Register the authorization handlers</span></span>

<span data-ttu-id="b67d2-207">Servicios mediante Entity Framework Core deben estar registrados para [inyección de dependencia](xref:fundamentals/dependency-injection) con [AddScoped](/aspnet/core/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span><span class="sxs-lookup"><span data-stu-id="b67d2-207">Services using Entity Framework Core must be registered for [dependency injection](xref:fundamentals/dependency-injection) using [AddScoped](/aspnet/core/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span></span> <span data-ttu-id="b67d2-208">El `ContactIsOwnerAuthorizationHandler` usa ASP.NET Core [identidad](xref:security/authentication/identity), que se basa en Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="b67d2-208">The `ContactIsOwnerAuthorizationHandler` uses ASP.NET Core [Identity](xref:security/authentication/identity), which is built on Entity Framework Core.</span></span> <span data-ttu-id="b67d2-209">Registre los controladores con la colección de servicio para que estén disponibles para la `ContactsController` a través de [inyección de dependencia](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="b67d2-209">Register the handlers with the service collection so they're available to the `ContactsController` through [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="b67d2-210">Agregue el código siguiente al final de `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="b67d2-210">Add the following code to the end of `ConfigureServices`:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Startup.cs?name=ConfigureServices&highlight=41-999)]

<span data-ttu-id="b67d2-211">`ContactAdministratorsAuthorizationHandler` y `ContactManagerAuthorizationHandler` se agregan como singleton.</span><span class="sxs-lookup"><span data-stu-id="b67d2-211">`ContactAdministratorsAuthorizationHandler` and `ContactManagerAuthorizationHandler` are added as singletons.</span></span> <span data-ttu-id="b67d2-212">Son singletons porque no usan EF y toda la información necesaria se encuentra en la `Context` parámetro de la `HandleRequirementAsync` método.</span><span class="sxs-lookup"><span data-stu-id="b67d2-212">They're singletons because they don't use EF and all the information needed is in the `Context` parameter of the `HandleRequirementAsync` method.</span></span>

## <a name="support-authorization"></a><span data-ttu-id="b67d2-213">Admitir la autorización</span><span class="sxs-lookup"><span data-stu-id="b67d2-213">Support authorization</span></span>

<span data-ttu-id="b67d2-214">En esta sección, actualice las páginas Razor y agregar una clase de requisitos de las operaciones.</span><span class="sxs-lookup"><span data-stu-id="b67d2-214">In this section, you update the Razor Pages and add an operations requirements class.</span></span>

### <a name="review-the-contact-operations-requirements-class"></a><span data-ttu-id="b67d2-215">Revisión de la clase de requisitos de operaciones de contacto</span><span class="sxs-lookup"><span data-stu-id="b67d2-215">Review the contact operations requirements class</span></span>

<span data-ttu-id="b67d2-216">Revise la `ContactOperations` clase.</span><span class="sxs-lookup"><span data-stu-id="b67d2-216">Review the `ContactOperations` class.</span></span> <span data-ttu-id="b67d2-217">Esta clase contiene los requisitos de la aplicación admite:</span><span class="sxs-lookup"><span data-stu-id="b67d2-217">This class contains the requirements the app supports:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-razor-pages"></a><span data-ttu-id="b67d2-218">Crear una clase base para las páginas de Razor</span><span class="sxs-lookup"><span data-stu-id="b67d2-218">Create a base class for the Razor Pages</span></span>

<span data-ttu-id="b67d2-219">Cree una clase base que contiene los servicios usados en los contactos de las páginas de Razor.</span><span class="sxs-lookup"><span data-stu-id="b67d2-219">Create a base class that contains the services used in the contacts Razor Pages.</span></span> <span data-ttu-id="b67d2-220">La clase base coloca ese código de inicialización en una ubicación:</span><span class="sxs-lookup"><span data-stu-id="b67d2-220">The base class puts that initialization code in one location:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/DI_BasePageModel.cs)]

<span data-ttu-id="b67d2-221">El código anterior:</span><span class="sxs-lookup"><span data-stu-id="b67d2-221">The preceding code:</span></span>

* <span data-ttu-id="b67d2-222">Agrega el `IAuthorizationService` servicio para tener acceso a los controladores de autorización.</span><span class="sxs-lookup"><span data-stu-id="b67d2-222">Adds the `IAuthorizationService` service to access to the authorization handlers.</span></span>
* <span data-ttu-id="b67d2-223">Agrega la identidad `UserManager` servicio.</span><span class="sxs-lookup"><span data-stu-id="b67d2-223">Adds the Identity `UserManager` service.</span></span>
* <span data-ttu-id="b67d2-224">Agregue la `ApplicationDbContext`.</span><span class="sxs-lookup"><span data-stu-id="b67d2-224">Add the `ApplicationDbContext`.</span></span>

### <a name="update-the-createmodel"></a><span data-ttu-id="b67d2-225">Actualizar el CreateModel</span><span class="sxs-lookup"><span data-stu-id="b67d2-225">Update the CreateModel</span></span>

<span data-ttu-id="b67d2-226">Actualizar el constructor de modelo de página de create para usar la `DI_BasePageModel` clase base:</span><span class="sxs-lookup"><span data-stu-id="b67d2-226">Update the create page model constructor to use the `DI_BasePageModel` base class:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

<span data-ttu-id="b67d2-227">Actualización de la `CreateModel.OnPostAsync` método:</span><span class="sxs-lookup"><span data-stu-id="b67d2-227">Update the `CreateModel.OnPostAsync` method to:</span></span>

* <span data-ttu-id="b67d2-228">Agregar el identificador de usuario para el `Contact` modelo.</span><span class="sxs-lookup"><span data-stu-id="b67d2-228">Add the user ID to the `Contact` model.</span></span>
* <span data-ttu-id="b67d2-229">Llamar al controlador de autorización para comprobar que el usuario tiene permiso para crear contactos.</span><span class="sxs-lookup"><span data-stu-id="b67d2-229">Call the authorization handler to verify the user has permission to create contacts.</span></span>

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a><span data-ttu-id="b67d2-230">Actualizar la IndexModel</span><span class="sxs-lookup"><span data-stu-id="b67d2-230">Update the IndexModel</span></span>

<span data-ttu-id="b67d2-231">Actualización de la `OnGetAsync` método por lo que sólo los contactos aprobados se muestran a los usuarios en general:</span><span class="sxs-lookup"><span data-stu-id="b67d2-231">Update the `OnGetAsync` method so only approved contacts are shown to general users:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a><span data-ttu-id="b67d2-232">Actualizar la EditModel</span><span class="sxs-lookup"><span data-stu-id="b67d2-232">Update the EditModel</span></span>

<span data-ttu-id="b67d2-233">Agregue un controlador de autorización para comprobar que el usuario es propietario del contacto.</span><span class="sxs-lookup"><span data-stu-id="b67d2-233">Add an authorization handler to verify the user owns the contact.</span></span> <span data-ttu-id="b67d2-234">Dado que se está validando la autorización de recursos, el `[Authorize]` atributo no es suficiente.</span><span class="sxs-lookup"><span data-stu-id="b67d2-234">Because resource authorization is being validated, the `[Authorize]` attribute is not enough.</span></span> <span data-ttu-id="b67d2-235">La aplicación no tiene acceso al recurso cuando se evalúan los atributos.</span><span class="sxs-lookup"><span data-stu-id="b67d2-235">The app doesn't have access to the resource when attributes are evaluated.</span></span> <span data-ttu-id="b67d2-236">Autorización basada en recursos debe ser imperativo.</span><span class="sxs-lookup"><span data-stu-id="b67d2-236">Resource-based authorization must be imperative.</span></span> <span data-ttu-id="b67d2-237">Las comprobaciones deben realizarse una vez que la aplicación tenga acceso al recurso, cargándolo en el modelo de páginas o cargándolo en su propio controlador.</span><span class="sxs-lookup"><span data-stu-id="b67d2-237">Checks must be performed once the app has access to the resource, either by loading it in the page model or by loading it within the handler itself.</span></span> <span data-ttu-id="b67d2-238">Con frecuencia tener acceso al recurso pasando la clave de recurso.</span><span class="sxs-lookup"><span data-stu-id="b67d2-238">You frequently access the resource by passing in the resource key.</span></span>

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a><span data-ttu-id="b67d2-239">Actualizar la DeleteModel</span><span class="sxs-lookup"><span data-stu-id="b67d2-239">Update the DeleteModel</span></span>

<span data-ttu-id="b67d2-240">Actualizar el modelo de páginas de delete para usar el controlador de autorización para comprobar que el usuario tiene permiso delete en el contacto.</span><span class="sxs-lookup"><span data-stu-id="b67d2-240">Update the delete page model to use the authorization handler to verify the user has delete permission on the contact.</span></span>

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a><span data-ttu-id="b67d2-241">Insertar el servicio de autorización en las vistas</span><span class="sxs-lookup"><span data-stu-id="b67d2-241">Inject the authorization service into the views</span></span>

<span data-ttu-id="b67d2-242">Actualmente, se muestra en la interfaz de usuario edita y elimina los vínculos de datos que no se puede modificar el usuario.</span><span class="sxs-lookup"><span data-stu-id="b67d2-242">Currently, the UI shows edit and delete links for data the user can't modify.</span></span> <span data-ttu-id="b67d2-243">La interfaz de usuario es fijo aplicando el controlador de autorización a las vistas.</span><span class="sxs-lookup"><span data-stu-id="b67d2-243">The UI is fixed by applying the authorization handler to the views.</span></span>

<span data-ttu-id="b67d2-244">Insertar el servicio de autorización en el *Views/_ViewImports.cshtml* para que esté disponible para todas las vistas de archivos:</span><span class="sxs-lookup"><span data-stu-id="b67d2-244">Inject the authorization service in the *Views/_ViewImports.cshtml* file so it's available to all views:</span></span>

[!code-cshtml[Main](secure-data/samples/final2/Pages/_ViewImports.cshtml?highlight=6-9)]

<span data-ttu-id="b67d2-245">El marcado anterior agrega varias `using` instrucciones.</span><span class="sxs-lookup"><span data-stu-id="b67d2-245">The preceding markup adds several `using` statements.</span></span>

<span data-ttu-id="b67d2-246">Actualización de la **editar** y **eliminar** vincula *Pages/Contacts/Index.cshtml* por lo que solo se procesan para los usuarios con los permisos adecuados:</span><span class="sxs-lookup"><span data-stu-id="b67d2-246">Update the **Edit** and **Delete** links in *Pages/Contacts/Index.cshtml* so they're only rendered for users with the appropriate permissions:</span></span>

[!code-cshtml[Main](secure-data/samples/final2/Pages/Contacts/Index.cshtml?highlight=34-36,64-999)]

> [!WARNING]
> <span data-ttu-id="b67d2-247">Ocultar los vínculos de los usuarios que no tienen permiso para cambiar los datos no proteger la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b67d2-247">Hiding links from users that don't have permission to change data doesn't secure the app.</span></span> <span data-ttu-id="b67d2-248">Ocultar los vínculos hace que la aplicación más fácil de usar mostrando vínculos solo es válido.</span><span class="sxs-lookup"><span data-stu-id="b67d2-248">Hiding links makes the app more user-friendly by displaying only valid links.</span></span> <span data-ttu-id="b67d2-249">Los usuarios pueden hack las direcciones URL generadas para invocar Editar y eliminar operaciones en los datos que no poseen.</span><span class="sxs-lookup"><span data-stu-id="b67d2-249">Users can hack the generated URLs to invoke edit and delete operations on data they don't own.</span></span> <span data-ttu-id="b67d2-250">La página de Razor o el controlador debe exigir comprobaciones de acceso para proteger los datos.</span><span class="sxs-lookup"><span data-stu-id="b67d2-250">The Razor Page or controller must enforce access checks to secure the data.</span></span>

### <a name="update-details"></a><span data-ttu-id="b67d2-251">Detalles de la actualización</span><span class="sxs-lookup"><span data-stu-id="b67d2-251">Update Details</span></span>

<span data-ttu-id="b67d2-252">Actualice la vista de detalles para que los administradores pueden aprobar o rechazar contactos:</span><span class="sxs-lookup"><span data-stu-id="b67d2-252">Update the details view so managers can approve or reject contacts:</span></span>

[!code-cshtml[Main](secure-data/samples/final2/Pages/Contacts/Details.cshtml?range=48-999)]

<span data-ttu-id="b67d2-253">Actualice el modelo de páginas de detalles:</span><span class="sxs-lookup"><span data-stu-id="b67d2-253">Update the details page model:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="test-the-completed-app"></a><span data-ttu-id="b67d2-254">Probar la aplicación completada</span><span class="sxs-lookup"><span data-stu-id="b67d2-254">Test the completed app</span></span>

<span data-ttu-id="b67d2-255">Si está usando Visual Studio Code o pruebas en una plataforma local que no incluye un certificado de prueba para HTTPS:</span><span class="sxs-lookup"><span data-stu-id="b67d2-255">If you're using Visual Studio Code or testing on a local platform that doesn't include a test certificate for HTTPS:</span></span>

* <span data-ttu-id="b67d2-256">Establecer `"LocalTest:skipSSL": true` en el *appsettings. Developement.JSON* archivo para omitir el requisito de HTTPS.</span><span class="sxs-lookup"><span data-stu-id="b67d2-256">Set `"LocalTest:skipSSL": true` in the *appsettings.Developement.json* file to skip the HTTPS requirement.</span></span> <span data-ttu-id="b67d2-257">Skip HTTPS solamente en un equipo de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="b67d2-257">Skip HTTPS only on a development machine.</span></span>

<span data-ttu-id="b67d2-258">Si la aplicación tiene contactos:</span><span class="sxs-lookup"><span data-stu-id="b67d2-258">If the app has contacts:</span></span>

* <span data-ttu-id="b67d2-259">Eliminar todos los registros en la `Contact` tabla.</span><span class="sxs-lookup"><span data-stu-id="b67d2-259">Delete all the records in the `Contact` table.</span></span>
* <span data-ttu-id="b67d2-260">Reinicie la aplicación para inicializar la base de datos.</span><span class="sxs-lookup"><span data-stu-id="b67d2-260">Restart the app to seed the database.</span></span>

<span data-ttu-id="b67d2-261">Registrar un usuario para examinar los contactos.</span><span class="sxs-lookup"><span data-stu-id="b67d2-261">Register a user for browsing the contacts.</span></span>

<span data-ttu-id="b67d2-262">Una manera sencilla de probar la aplicación final consiste en iniciar tres distintos exploradores (o versiones de incógnito/InPrivate).</span><span class="sxs-lookup"><span data-stu-id="b67d2-262">An easy way to test the completed app is to launch three different browsers (or incognito/InPrivate versions).</span></span> <span data-ttu-id="b67d2-263">En un explorador, registrar un nuevo usuario (por ejemplo, `test@example.com`).</span><span class="sxs-lookup"><span data-stu-id="b67d2-263">In one browser, register a new user (for example, `test@example.com`).</span></span> <span data-ttu-id="b67d2-264">Inicie sesión con un usuario diferente en cada explorador.</span><span class="sxs-lookup"><span data-stu-id="b67d2-264">Sign in to each browser with a different user.</span></span> <span data-ttu-id="b67d2-265">Compruebe las operaciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="b67d2-265">Verify the following operations:</span></span>

* <span data-ttu-id="b67d2-266">Los usuarios registrados pueden ver todos los datos de contacto aprobados.</span><span class="sxs-lookup"><span data-stu-id="b67d2-266">Registered users can view all the approved contact data.</span></span>
* <span data-ttu-id="b67d2-267">Los usuarios registrados pueden editar o eliminar sus propios datos.</span><span class="sxs-lookup"><span data-stu-id="b67d2-267">Registered users can edit/delete their own data.</span></span>
* <span data-ttu-id="b67d2-268">Los administradores pueden aprobar o rechazar datos de contacto.</span><span class="sxs-lookup"><span data-stu-id="b67d2-268">Managers can approve or reject contact data.</span></span> <span data-ttu-id="b67d2-269">El `Details` vista muestra **aprobar** y **rechazar** botones.</span><span class="sxs-lookup"><span data-stu-id="b67d2-269">The `Details` view shows **Approve** and **Reject** buttons.</span></span>
* <span data-ttu-id="b67d2-270">Los administradores pueden aprobar o rechazar y editar o eliminar los datos.</span><span class="sxs-lookup"><span data-stu-id="b67d2-270">Administrators can approve/reject and edit/delete any data.</span></span>

| <span data-ttu-id="b67d2-271">Usuario</span><span class="sxs-lookup"><span data-stu-id="b67d2-271">User</span></span>| <span data-ttu-id="b67d2-272">Opciones</span><span class="sxs-lookup"><span data-stu-id="b67d2-272">Options</span></span> |
| ------------ | ---------|
| test@example.com | <span data-ttu-id="b67d2-273">Editar puede/eliminar datos propios</span><span class="sxs-lookup"><span data-stu-id="b67d2-273">Can edit/delete own data</span></span> |
| manager@contoso.com | <span data-ttu-id="b67d2-274">Pueden aprobar o rechazar y editar o eliminar datos de propietario</span><span class="sxs-lookup"><span data-stu-id="b67d2-274">Can approve/reject and edit/delete own data</span></span> |
| admin@contoso.com | <span data-ttu-id="b67d2-275">Puede editar o eliminar y aprobar o rechazar todos los datos</span><span class="sxs-lookup"><span data-stu-id="b67d2-275">Can edit/delete and approve/reject all data</span></span>|

<span data-ttu-id="b67d2-276">Crear un contacto en el explorador del administrador.</span><span class="sxs-lookup"><span data-stu-id="b67d2-276">Create a contact in the administrator's browser.</span></span> <span data-ttu-id="b67d2-277">Copie la dirección URL para eliminar y editar en el contacto del administrador.</span><span class="sxs-lookup"><span data-stu-id="b67d2-277">Copy the URL for delete and edit from the administrator contact.</span></span> <span data-ttu-id="b67d2-278">Pegue estos vínculos en el explorador del usuario de prueba para comprobar que el usuario de prueba no puede realizar estas operaciones.</span><span class="sxs-lookup"><span data-stu-id="b67d2-278">Paste these links into the test user's browser to verify the test user can't perform these operations.</span></span>

## <a name="create-the-starter-app"></a><span data-ttu-id="b67d2-279">Crear la aplicación de inicio</span><span class="sxs-lookup"><span data-stu-id="b67d2-279">Create the starter app</span></span>

* <span data-ttu-id="b67d2-280">Crear una aplicación de páginas de Razor denominada "ContactManager"</span><span class="sxs-lookup"><span data-stu-id="b67d2-280">Create a Razor Pages app named "ContactManager"</span></span>

  * <span data-ttu-id="b67d2-281">Crear la aplicación con **cuentas de usuario individuales**.</span><span class="sxs-lookup"><span data-stu-id="b67d2-281">Create the app with **Individual User Accounts**.</span></span>
  * <span data-ttu-id="b67d2-282">Asígnele el nombre "ContactManager" para el espacio de nombres coincide con el espacio de nombres utilizado en el ejemplo.</span><span class="sxs-lookup"><span data-stu-id="b67d2-282">Name it "ContactManager" so your namespace matches the namespace used in the sample.</span></span>

  ```console
  dotnet new razor -o ContactManager -au Individual -uld
  ```

  * <span data-ttu-id="b67d2-283">`-uld` Especifica LocalDB en lugar de SQLite</span><span class="sxs-lookup"><span data-stu-id="b67d2-283">`-uld` specifies LocalDB instead of SQLite</span></span>

* <span data-ttu-id="b67d2-284">Agregue las siguientes `Contact` modelo:</span><span class="sxs-lookup"><span data-stu-id="b67d2-284">Add the following `Contact` model:</span></span>

  [!code-csharp[Main](secure-data/samples/starter2/Models/Contact.cs?name=snippet1)]

* <span data-ttu-id="b67d2-285">Scaffold el `Contact` modelo:</span><span class="sxs-lookup"><span data-stu-id="b67d2-285">Scaffold the `Contact` model:</span></span>

```console
dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
```

* <span data-ttu-id="b67d2-286">Actualización de la **ContactManager** fijar en el *Pages/_Layout.cshtml* archivo:</span><span class="sxs-lookup"><span data-stu-id="b67d2-286">Update the **ContactManager** anchor in the *Pages/_Layout.cshtml* file:</span></span>

```cshtml
<a asp-page="/Contacts/Index" class="navbar-brand">ContactManager</a>
```

* <span data-ttu-id="b67d2-287">Aplicar la técnica scaffolding la migración inicial y actualizar la base de datos:</span><span class="sxs-lookup"><span data-stu-id="b67d2-287">Scaffold the initial migration and update the database:</span></span>

```console
dotnet ef migrations add initial
dotnet ef database update
```

* <span data-ttu-id="b67d2-288">Probar la aplicación mediante la creación, edición y eliminación de un contacto</span><span class="sxs-lookup"><span data-stu-id="b67d2-288">Test the app by creating, editing, and deleting a contact</span></span>

### <a name="seed-the-database"></a><span data-ttu-id="b67d2-289">Inicializar la base de datos</span><span class="sxs-lookup"><span data-stu-id="b67d2-289">Seed the database</span></span>

<span data-ttu-id="b67d2-290">Agregar el `SeedData` clase a la *datos* carpeta.</span><span class="sxs-lookup"><span data-stu-id="b67d2-290">Add the `SeedData` class to the *Data* folder.</span></span> <span data-ttu-id="b67d2-291">Si ha descargado el ejemplo, puede copiar la *SeedData.cs* del archivo a la *datos* carpeta del proyecto de inicio.</span><span class="sxs-lookup"><span data-stu-id="b67d2-291">If you've downloaded the sample, you can copy the *SeedData.cs* file to the *Data* folder of the starter project.</span></span>

<span data-ttu-id="b67d2-292">Llame a `SeedData.Initialize` de `Main`:</span><span class="sxs-lookup"><span data-stu-id="b67d2-292">Call `SeedData.Initialize` from `Main`:</span></span>

[!code-csharp[Main](secure-data/samples/starter2/Program.cs?name=snippet)]

<span data-ttu-id="b67d2-293">Compruebe que la aplicación había propagado la base de datos.</span><span class="sxs-lookup"><span data-stu-id="b67d2-293">Test that the app seeded the database.</span></span> <span data-ttu-id="b67d2-294">Si no hay ninguna fila en la base de datos de contacto, no se ejecuta el método de inicialización.</span><span class="sxs-lookup"><span data-stu-id="b67d2-294">If there are any rows in the contact DB, the seed method doesn't run.</span></span>

<a name="secure-data-add-resources-label"></a>

### <a name="additional-resources"></a><span data-ttu-id="b67d2-295">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="b67d2-295">Additional resources</span></span>

* <span data-ttu-id="b67d2-296">[Laboratorio de autorización de ASP.NET Core](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span><span class="sxs-lookup"><span data-stu-id="b67d2-296">[ASP.NET Core Authorization Lab](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span></span> <span data-ttu-id="b67d2-297">Este laboratorio se explica con más detalle en las características de seguridad introducidas en este tutorial.</span><span class="sxs-lookup"><span data-stu-id="b67d2-297">This lab goes into more detail on the security features introduced in this tutorial.</span></span>
* [<span data-ttu-id="b67d2-298">Autorización de ASP.NET Core: Simple, rol, basada en notificaciones y personalizado</span><span class="sxs-lookup"><span data-stu-id="b67d2-298">Authorization in ASP.NET Core: Simple, role, claims-based, and custom</span></span>](xref:security/authorization/index)
* [<span data-ttu-id="b67d2-299">Autorización personalizada basada en directivas</span><span class="sxs-lookup"><span data-stu-id="b67d2-299">Custom policy-based authorization</span></span>](xref:security/authorization/policies)
