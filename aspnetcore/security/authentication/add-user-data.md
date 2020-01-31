---
title: Agregar, descargar y eliminar datos de usuario a la identidad en un proyecto de ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo agregar datos de usuario personalizada para la identidad en un proyecto de ASP.NET Core. Eliminar datos de RGPD.
ms.author: riande
ms.date: 01/28/2020
ms.custom: mvc, seodec18
uid: security/authentication/add-user-data
ms.openlocfilehash: e08c02e2e5d4a429aae10c59e7ae3ea48c975067
ms.sourcegitcommit: c81ef12a1b6e6ac838e5e07042717cf492e6635b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2020
ms.locfileid: "76885544"
---
# <a name="add-download-and-delete-custom-user-data-to-identity-in-an-aspnet-core-project"></a><span data-ttu-id="60260-104">Agregar, descargar y eliminar datos de usuario personalizado a la identidad en un proyecto de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="60260-104">Add, download, and delete custom user data to Identity in an ASP.NET Core project</span></span>

<span data-ttu-id="60260-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="60260-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="60260-106">Este artículo se muestra cómo:</span><span class="sxs-lookup"><span data-stu-id="60260-106">This article shows how to:</span></span>

* <span data-ttu-id="60260-107">Agregar datos de usuario personalizado a una aplicación web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="60260-107">Add custom user data to an ASP.NET Core web app.</span></span>
* <span data-ttu-id="60260-108">Marque el modelo de datos de usuario personalizado con el <xref:Microsoft.AspNetCore.Identity.PersonalDataAttribute> atributo para que esté disponible automáticamente para su descarga y eliminación.</span><span class="sxs-lookup"><span data-stu-id="60260-108">Mark the custom user data model with the <xref:Microsoft.AspNetCore.Identity.PersonalDataAttribute> attribute so it's automatically available for download and deletion.</span></span> <span data-ttu-id="60260-109">Hacer que los datos que puede descargarse y eliminado ayuda a cumplir [RGPD](xref:security/gdpr) requisitos.</span><span class="sxs-lookup"><span data-stu-id="60260-109">Making the data able to be downloaded and deleted helps meet [GDPR](xref:security/gdpr) requirements.</span></span>

<span data-ttu-id="60260-110">Se crea el proyecto de ejemplo desde una aplicación web de páginas de Razor, pero las instrucciones son similares para una aplicación web de ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="60260-110">The project sample is created from a Razor Pages web app, but the instructions are similar for a ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="60260-111">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/add-user-data) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="60260-111">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/add-user-data) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="60260-112">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="60260-112">Prerequisites</span></span>

::: moniker range=">= aspnetcore-3.0"

[!INCLUDE [](~/includes/3.0-SDK.md)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!INCLUDE [](~/includes/2.2-SDK.md)]

::: moniker-end

## <a name="create-a-razor-web-app"></a><span data-ttu-id="60260-113">Creación de una aplicación web de Razor</span><span class="sxs-lookup"><span data-stu-id="60260-113">Create a Razor web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="60260-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="60260-114">Visual Studio</span></span>](#tab/visual-studio)

::: moniker range=">= aspnetcore-3.0"

* <span data-ttu-id="60260-115">En el menú **Archivo** de Visual Studio, seleccione **Nuevo** > **Proyecto**.</span><span class="sxs-lookup"><span data-stu-id="60260-115">From the Visual Studio **File** menu, select **New** > **Project**.</span></span> <span data-ttu-id="60260-116">Denomine el proyecto **WebApp1** si desea coincida con el espacio de nombres de los [Descargar ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/authentication/add-user-data) código.</span><span class="sxs-lookup"><span data-stu-id="60260-116">Name the project **WebApp1** if you want to it match the namespace of the [download sample](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/authentication/add-user-data) code.</span></span>
* <span data-ttu-id="60260-117">Seleccione **ASP.net Core aplicación Web** > **Aceptar** .</span><span class="sxs-lookup"><span data-stu-id="60260-117">Select **ASP.NET Core Web Application** > **OK**</span></span>
* <span data-ttu-id="60260-118">Seleccione **ASP.NET Core 3,0** en la lista desplegable.</span><span class="sxs-lookup"><span data-stu-id="60260-118">Select **ASP.NET Core 3.0** in the dropdown</span></span>
* <span data-ttu-id="60260-119">Seleccionar **aplicación Web** > **Aceptar**</span><span class="sxs-lookup"><span data-stu-id="60260-119">Select **Web Application** > **OK**</span></span>
* <span data-ttu-id="60260-120">Cree y ejecute el proyecto.</span><span class="sxs-lookup"><span data-stu-id="60260-120">Build and run the project.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* <span data-ttu-id="60260-121">En el menú **Archivo** de Visual Studio, seleccione **Nuevo** > **Proyecto**.</span><span class="sxs-lookup"><span data-stu-id="60260-121">From the Visual Studio **File** menu, select **New** > **Project**.</span></span> <span data-ttu-id="60260-122">Denomine el proyecto **WebApp1** si desea coincida con el espacio de nombres de los [Descargar ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/authentication/add-user-data) código.</span><span class="sxs-lookup"><span data-stu-id="60260-122">Name the project **WebApp1** if you want to it match the namespace of the [download sample](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/authentication/add-user-data) code.</span></span>
* <span data-ttu-id="60260-123">Seleccione **ASP.net Core aplicación Web** > **Aceptar** .</span><span class="sxs-lookup"><span data-stu-id="60260-123">Select **ASP.NET Core Web Application** > **OK**</span></span>
* <span data-ttu-id="60260-124">Seleccione **ASP.NET Core 2,2** en la lista desplegable.</span><span class="sxs-lookup"><span data-stu-id="60260-124">Select **ASP.NET Core 2.2** in the dropdown</span></span>
* <span data-ttu-id="60260-125">Seleccionar **aplicación Web** > **Aceptar**</span><span class="sxs-lookup"><span data-stu-id="60260-125">Select **Web Application** > **OK**</span></span>
* <span data-ttu-id="60260-126">Cree y ejecute el proyecto.</span><span class="sxs-lookup"><span data-stu-id="60260-126">Build and run the project.</span></span>

::: moniker-end


# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="60260-127">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="60260-127">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet new webapp -o WebApp1
```

---

## <a name="run-the-identity-scaffolder"></a><span data-ttu-id="60260-128">Ejecute el proveedor de scaffolding de identidad</span><span class="sxs-lookup"><span data-stu-id="60260-128">Run the Identity scaffolder</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="60260-129">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="60260-129">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="60260-130">Desde **el Explorador de soluciones**, haga doble clic en el proyecto > **agregar** > **nuevo elemento de scaffolding**.</span><span class="sxs-lookup"><span data-stu-id="60260-130">From **Solution Explorer**, right-click on the project > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="60260-131">En el panel izquierdo del cuadro de diálogo **Agregar scaffold** , seleccione **Identity** > **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="60260-131">From the left pane of the **Add Scaffold** dialog, select **Identity** > **Add**.</span></span>
* <span data-ttu-id="60260-132">En el cuadro de diálogo **Agregar identidad** , las siguientes opciones:</span><span class="sxs-lookup"><span data-stu-id="60260-132">In the **Add Identity** dialog, the following options:</span></span>
  * <span data-ttu-id="60260-133">Seleccione el archivo de diseño existente *~/Pages/Shared/_Layout.cshtml*</span><span class="sxs-lookup"><span data-stu-id="60260-133">Select the existing layout  file  *~/Pages/Shared/_Layout.cshtml*</span></span>
  * <span data-ttu-id="60260-134">Seleccione los archivos siguientes para reemplazar:</span><span class="sxs-lookup"><span data-stu-id="60260-134">Select the following files to override:</span></span>
    * <span data-ttu-id="60260-135">**Cuenta/Register**</span><span class="sxs-lookup"><span data-stu-id="60260-135">**Account/Register**</span></span>
    * <span data-ttu-id="60260-136">**Cuenta/administrar o índice**</span><span class="sxs-lookup"><span data-stu-id="60260-136">**Account/Manage/Index**</span></span>
  * <span data-ttu-id="60260-137">Seleccione el **+** botón para crear un nuevo **clase de contexto de datos**.</span><span class="sxs-lookup"><span data-stu-id="60260-137">Select the **+** button to create a new **Data context class**.</span></span> <span data-ttu-id="60260-138">Acepte el tipo (**WebApp1.Models.WebApp1Context** si el proyecto se denomina **WebApp1**).</span><span class="sxs-lookup"><span data-stu-id="60260-138">Accept the type (**WebApp1.Models.WebApp1Context** if the project is named **WebApp1**).</span></span>
  * <span data-ttu-id="60260-139">Seleccione el **+** botón para crear un nuevo **clase User**.</span><span class="sxs-lookup"><span data-stu-id="60260-139">Select the **+** button to create a new **User class**.</span></span> <span data-ttu-id="60260-140">Acepte el tipo (**WebApp1User** si el proyecto se denomina **WebApp1**) > **agregar**.</span><span class="sxs-lookup"><span data-stu-id="60260-140">Accept the type (**WebApp1User** if the project is named **WebApp1**) > **Add**.</span></span>
* <span data-ttu-id="60260-141">Seleccione **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="60260-141">Select **Add**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="60260-142">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="60260-142">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="60260-143">Si no ha instalado previamente el proveedor de scaffolding de ASP.NET Core, instalar ahora:</span><span class="sxs-lookup"><span data-stu-id="60260-143">If you have not previously installed the ASP.NET Core scaffolder, install it now:</span></span>

```dotnetcli
dotnet tool install -g dotnet-aspnet-codegenerator
```

<span data-ttu-id="60260-144">Agregue una referencia de paquete a [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) al archivo de proyecto (.csproj).</span><span class="sxs-lookup"><span data-stu-id="60260-144">Add a package reference to [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) to the project (.csproj) file.</span></span> <span data-ttu-id="60260-145">Ejecute el siguiente comando en el directorio del proyecto:</span><span class="sxs-lookup"><span data-stu-id="60260-145">Run the following command in the project directory:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

<span data-ttu-id="60260-146">Ejecute el siguiente comando para enumerar las opciones de proveedor de scaffolding de identidad:</span><span class="sxs-lookup"><span data-stu-id="60260-146">Run the following command to list the Identity scaffolder options:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity -h
```

<span data-ttu-id="60260-147">En la carpeta del proyecto, ejecute el proveedor de scaffolding de identidad:</span><span class="sxs-lookup"><span data-stu-id="60260-147">In the project folder, run the Identity scaffolder:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity -u WebApp1User -fi Account.Register;Account.Manage.Index
```

---

<span data-ttu-id="60260-148">Siga las instrucciones [migraciones, UseAuthentication y diseño](xref:security/authentication/scaffold-identity#efm) para realizar los pasos siguientes:</span><span class="sxs-lookup"><span data-stu-id="60260-148">Follow the instruction in [Migrations, UseAuthentication, and layout](xref:security/authentication/scaffold-identity#efm) to perform the following steps:</span></span>

* <span data-ttu-id="60260-149">Cree una migración y actualización de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="60260-149">Create a migration and update the database.</span></span>
* <span data-ttu-id="60260-150">Agregue `UseAuthentication` a `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="60260-150">Add `UseAuthentication` to `Startup.Configure`.</span></span>
* <span data-ttu-id="60260-151">Agregar `<partial name="_LoginPartial" />` para el archivo de diseño.</span><span class="sxs-lookup"><span data-stu-id="60260-151">Add `<partial name="_LoginPartial" />` to the layout file.</span></span>
* <span data-ttu-id="60260-152">Pruebe la aplicación:</span><span class="sxs-lookup"><span data-stu-id="60260-152">Test the app:</span></span>
  * <span data-ttu-id="60260-153">Registrar un usuario</span><span class="sxs-lookup"><span data-stu-id="60260-153">Register a user</span></span>
  * <span data-ttu-id="60260-154">Seleccione el nuevo nombre de usuario (junto a la **Logout** vínculo).</span><span class="sxs-lookup"><span data-stu-id="60260-154">Select the new user name (next to the **Logout** link).</span></span> <span data-ttu-id="60260-155">Es posible que deba expandir la ventana o seleccione el icono de la barra de navegación para mostrar el nombre de usuario y otros vínculos.</span><span class="sxs-lookup"><span data-stu-id="60260-155">You might need to expand the window or select the navigation bar icon to show the user name and other links.</span></span>
  * <span data-ttu-id="60260-156">Seleccione el **datos personales** ficha.</span><span class="sxs-lookup"><span data-stu-id="60260-156">Select the **Personal Data** tab.</span></span>
  * <span data-ttu-id="60260-157">Seleccione el **descargar** botón y examinar el *PersonalData.json* archivo.</span><span class="sxs-lookup"><span data-stu-id="60260-157">Select the **Download** button and examined the *PersonalData.json* file.</span></span>
  * <span data-ttu-id="60260-158">Prueba la **eliminar** botón, lo que elimina el inicio de sesión de usuario.</span><span class="sxs-lookup"><span data-stu-id="60260-158">Test the **Delete** button, which deletes the logged on user.</span></span>

## <a name="add-custom-user-data-to-the-identity-db"></a><span data-ttu-id="60260-159">Agregar datos de usuario personalizado a la base de datos de identidad</span><span class="sxs-lookup"><span data-stu-id="60260-159">Add custom user data to the Identity DB</span></span>

<span data-ttu-id="60260-160">Actualización de la `IdentityUser` derivados de la clase con propiedades personalizadas.</span><span class="sxs-lookup"><span data-stu-id="60260-160">Update the `IdentityUser` derived class with custom properties.</span></span> <span data-ttu-id="60260-161">Si el nombre del proyecto WebApp1, el archivo se denomina *Areas/Identity/Data/WebApp1User.cs*.</span><span class="sxs-lookup"><span data-stu-id="60260-161">If you named the project WebApp1, the file is named *Areas/Identity/Data/WebApp1User.cs*.</span></span> <span data-ttu-id="60260-162">Actualice el archivo con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="60260-162">Update the file with the following code:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Data/WebApp1User.cs)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Data/WebApp1User.cs)]

::: moniker-end

<span data-ttu-id="60260-163">Las propiedades con el atributo [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute) son:</span><span class="sxs-lookup"><span data-stu-id="60260-163">Properties with the [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute) attribute are:</span></span>

* <span data-ttu-id="60260-164">Cuando elimina el *Areas/Identity/Pages/Account/Manage/DeletePersonalData.cshtml* llama la página de Razor `UserManager.Delete`.</span><span class="sxs-lookup"><span data-stu-id="60260-164">Deleted when the *Areas/Identity/Pages/Account/Manage/DeletePersonalData.cshtml* Razor Page calls `UserManager.Delete`.</span></span>
* <span data-ttu-id="60260-165">Incluye en los datos descargados por el *Areas/Identity/Pages/Account/Manage/DownloadPersonalData.cshtml* página de Razor.</span><span class="sxs-lookup"><span data-stu-id="60260-165">Included in the downloaded data by the *Areas/Identity/Pages/Account/Manage/DownloadPersonalData.cshtml* Razor Page.</span></span>

### <a name="update-the-accountmanageindexcshtml-page"></a><span data-ttu-id="60260-166">Actualizar la página Account/Manage/Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="60260-166">Update the Account/Manage/Index.cshtml page</span></span>

<span data-ttu-id="60260-167">Actualización de la `InputModel` en *Areas/Identity/Pages/Account/Manage/Index.cshtml.cs* con el siguiente código resaltado:</span><span class="sxs-lookup"><span data-stu-id="60260-167">Update the `InputModel` in *Areas/Identity/Pages/Account/Manage/Index.cshtml.cs* with the following highlighted code:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml.cs?name=snippet&highlight=24-32,48-49,96-104,106)]

<span data-ttu-id="60260-168">Actualización de la *Areas/Identity/Pages/Account/Manage/Index.cshtml* con el siguiente marcado resaltado:</span><span class="sxs-lookup"><span data-stu-id="60260-168">Update the *Areas/Identity/Pages/Account/Manage/Index.cshtml* with the following highlighted markup:</span></span>

[!code-cshtml[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml?highlight=18-25)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml.cs?name=snippet&highlight=28-36,63-64,98-106,119)]

<span data-ttu-id="60260-169">Actualización de la *Areas/Identity/Pages/Account/Manage/Index.cshtml* con el siguiente marcado resaltado:</span><span class="sxs-lookup"><span data-stu-id="60260-169">Update the *Areas/Identity/Pages/Account/Manage/Index.cshtml* with the following highlighted markup:</span></span>

[!code-chtml[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml?highlight=35-42)]

::: moniker-end

### <a name="update-the-accountregistercshtml-page"></a><span data-ttu-id="60260-170">Actualizar la página Account/Register.cshtml</span><span class="sxs-lookup"><span data-stu-id="60260-170">Update the Account/Register.cshtml page</span></span>

<span data-ttu-id="60260-171">Actualización de la `InputModel` en *Areas/Identity/Pages/Account/Register.cshtml.cs* con el siguiente código resaltado:</span><span class="sxs-lookup"><span data-stu-id="60260-171">Update the `InputModel` in *Areas/Identity/Pages/Account/Register.cshtml.cs* with the following highlighted code:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=30-38,70-71)]

<span data-ttu-id="60260-172">Actualización de la *Areas/Identity/Pages/Account/Register.cshtml* con el siguiente marcado resaltado:</span><span class="sxs-lookup"><span data-stu-id="60260-172">Update the *Areas/Identity/Pages/Account/Register.cshtml* with the following highlighted markup:</span></span>

[!code-cshtml[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml?highlight=16-25)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=28-36,67,66)]

<span data-ttu-id="60260-173">Actualización de la *Areas/Identity/Pages/Account/Register.cshtml* con el siguiente marcado resaltado:</span><span class="sxs-lookup"><span data-stu-id="60260-173">Update the *Areas/Identity/Pages/Account/Register.cshtml* with the following highlighted markup:</span></span>

[!code-chtml[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml?highlight=16-25)]

::: moniker-end


<span data-ttu-id="60260-174">Generar el proyecto.</span><span class="sxs-lookup"><span data-stu-id="60260-174">Build the project.</span></span>

### <a name="add-a-migration-for-the-custom-user-data"></a><span data-ttu-id="60260-175">Agrega una migración para los datos de usuario personalizada</span><span class="sxs-lookup"><span data-stu-id="60260-175">Add a migration for the custom user data</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="60260-176">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="60260-176">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="60260-177">En Visual Studio **Package Manager Console**:</span><span class="sxs-lookup"><span data-stu-id="60260-177">In the Visual Studio **Package Manager Console**:</span></span>

```powershell
Add-Migration CustomUserData
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="60260-178">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="60260-178">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet ef migrations add CustomUserData
dotnet ef database update
```

---

## <a name="test-create-view-download-delete-custom-user-data"></a><span data-ttu-id="60260-179">Prueba crear, ver, descargar y eliminar datos de usuario personalizada</span><span class="sxs-lookup"><span data-stu-id="60260-179">Test create, view, download, delete custom user data</span></span>

<span data-ttu-id="60260-180">Pruebe la aplicación:</span><span class="sxs-lookup"><span data-stu-id="60260-180">Test the app:</span></span>

* <span data-ttu-id="60260-181">Registrar un nuevo usuario.</span><span class="sxs-lookup"><span data-stu-id="60260-181">Register a new user.</span></span>
* <span data-ttu-id="60260-182">Ver los datos de usuario personalizada en el `/Identity/Account/Manage` página.</span><span class="sxs-lookup"><span data-stu-id="60260-182">View the custom user data on the `/Identity/Account/Manage` page.</span></span>
* <span data-ttu-id="60260-183">Descargar y ver los datos personales de los usuarios desde el `/Identity/Account/Manage/PersonalData` página.</span><span class="sxs-lookup"><span data-stu-id="60260-183">Download and view the users personal data from the `/Identity/Account/Manage/PersonalData` page.</span></span>
