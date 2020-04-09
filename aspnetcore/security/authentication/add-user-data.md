---
title: Agregue, descargue y elimine datos de usuario a Identity en un proyecto de ASP.NET Core
author: rick-anderson
description: Aprenda a agregar datos de usuario personalizados a Identity en un proyecto de ASP.NET Core. Eliminar datos por RGPD.
ms.author: riande
ms.date: 03/26/2020
ms.custom: mvc, seodec18
uid: security/authentication/add-user-data
ms.openlocfilehash: 76b83df22381429feab80056c36dbdac1e5f20c7
ms.sourcegitcommit: 1d8f1396ccc66a0c3fcb5e5f36ea29b50db6d92a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/01/2020
ms.locfileid: "80501226"
---
# <a name="add-download-and-delete-custom-user-data-to-identity-in-an-aspnet-core-project"></a><span data-ttu-id="acf53-104">Agregue, descargue y elimine datos de usuario personalizados a Identity en un proyecto de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="acf53-104">Add, download, and delete custom user data to Identity in an ASP.NET Core project</span></span>

<span data-ttu-id="acf53-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="acf53-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="acf53-106">En este artículo se muestra cómo:</span><span class="sxs-lookup"><span data-stu-id="acf53-106">This article shows how to:</span></span>

* <span data-ttu-id="acf53-107">Agregue datos de usuario personalizados a una aplicación web de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="acf53-107">Add custom user data to an ASP.NET Core web app.</span></span>
* <span data-ttu-id="acf53-108">Marque el modelo de <xref:Microsoft.AspNetCore.Identity.PersonalDataAttribute> datos de usuario personalizado con el atributo para que esté disponible automáticamente para su descarga y eliminación.</span><span class="sxs-lookup"><span data-stu-id="acf53-108">Mark the custom user data model with the <xref:Microsoft.AspNetCore.Identity.PersonalDataAttribute> attribute so it's automatically available for download and deletion.</span></span> <span data-ttu-id="acf53-109">Hacer que los datos puedan descargarse y eliminarse ayuda a cumplir los requisitos [del RGPD.](xref:security/gdpr)</span><span class="sxs-lookup"><span data-stu-id="acf53-109">Making the data able to be downloaded and deleted helps meet [GDPR](xref:security/gdpr) requirements.</span></span>

<span data-ttu-id="acf53-110">El ejemplo de proyecto se crea a partir de una aplicación web razor Pages, pero las instrucciones son similares para una aplicación web de ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="acf53-110">The project sample is created from a Razor Pages web app, but the instructions are similar for a ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="acf53-111">[Ver o descargar código de ejemplo](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/add-user-data) ( cómo[descargar](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="acf53-111">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/add-user-data) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="acf53-112">Prerrequisitos</span><span class="sxs-lookup"><span data-stu-id="acf53-112">Prerequisites</span></span>

::: moniker range=">= aspnetcore-3.0"

[!INCLUDE [](~/includes/3.0-SDK.md)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!INCLUDE [](~/includes/2.2-SDK.md)]

::: moniker-end

## <a name="create-a-razor-web-app"></a><span data-ttu-id="acf53-113">Creación de una aplicación web de Razor</span><span class="sxs-lookup"><span data-stu-id="acf53-113">Create a Razor web app</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="acf53-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="acf53-114">Visual Studio</span></span>](#tab/visual-studio)

::: moniker range=">= aspnetcore-3.0"

* <span data-ttu-id="acf53-115">En el menú **Archivo** de Visual Studio, seleccione **Nuevo** > **Proyecto**.</span><span class="sxs-lookup"><span data-stu-id="acf53-115">From the Visual Studio **File** menu, select **New** > **Project**.</span></span> <span data-ttu-id="acf53-116">Asigne al proyecto el nombre **WebApp1** si desea que coincida con el espacio de nombres del código de ejemplo de [descarga.](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/authentication/add-user-data)</span><span class="sxs-lookup"><span data-stu-id="acf53-116">Name the project **WebApp1** if you want to it match the namespace of the [download sample](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/authentication/add-user-data) code.</span></span>
* <span data-ttu-id="acf53-117">Seleccione **ASP.NET aplicación** > web principal **ACEPTAR**</span><span class="sxs-lookup"><span data-stu-id="acf53-117">Select **ASP.NET Core Web Application** > **OK**</span></span>
* <span data-ttu-id="acf53-118">Seleccione **ASP.NET Core 3.0** en el menú desplegable</span><span class="sxs-lookup"><span data-stu-id="acf53-118">Select **ASP.NET Core 3.0** in the dropdown</span></span>
* <span data-ttu-id="acf53-119">Seleccione **Aplicación** > web **OK**</span><span class="sxs-lookup"><span data-stu-id="acf53-119">Select **Web Application** > **OK**</span></span>
* <span data-ttu-id="acf53-120">Compile y ejecute el proyecto.</span><span class="sxs-lookup"><span data-stu-id="acf53-120">Build and run the project.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* <span data-ttu-id="acf53-121">En el menú **Archivo** de Visual Studio, seleccione **Nuevo** > **Proyecto**.</span><span class="sxs-lookup"><span data-stu-id="acf53-121">From the Visual Studio **File** menu, select **New** > **Project**.</span></span> <span data-ttu-id="acf53-122">Asigne al proyecto el nombre **WebApp1** si desea que coincida con el espacio de nombres del código de ejemplo de [descarga.](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/authentication/add-user-data)</span><span class="sxs-lookup"><span data-stu-id="acf53-122">Name the project **WebApp1** if you want to it match the namespace of the [download sample](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/authentication/add-user-data) code.</span></span>
* <span data-ttu-id="acf53-123">Seleccione **ASP.NET aplicación** > web principal **ACEPTAR**</span><span class="sxs-lookup"><span data-stu-id="acf53-123">Select **ASP.NET Core Web Application** > **OK**</span></span>
* <span data-ttu-id="acf53-124">Seleccione **ASP.NET Core 2.2** en el menú desplegable</span><span class="sxs-lookup"><span data-stu-id="acf53-124">Select **ASP.NET Core 2.2** in the dropdown</span></span>
* <span data-ttu-id="acf53-125">Seleccione **Aplicación** > web **OK**</span><span class="sxs-lookup"><span data-stu-id="acf53-125">Select **Web Application** > **OK**</span></span>
* <span data-ttu-id="acf53-126">Compile y ejecute el proyecto.</span><span class="sxs-lookup"><span data-stu-id="acf53-126">Build and run the project.</span></span>

::: moniker-end


# <a name="net-core-cli"></a>[<span data-ttu-id="acf53-127">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="acf53-127">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet new webapp -o WebApp1
```

---

## <a name="run-the-identity-scaffolder"></a><span data-ttu-id="acf53-128">Ejecute el scaffolder Identity</span><span class="sxs-lookup"><span data-stu-id="acf53-128">Run the Identity scaffolder</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="acf53-129">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="acf53-129">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="acf53-130">En **el Explorador**de soluciones , haga clic con el botón derecho en el proyecto > **Agregar** > **nuevo elemento con scaffolding**.</span><span class="sxs-lookup"><span data-stu-id="acf53-130">From **Solution Explorer**, right-click on the project > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="acf53-131">En el panel izquierdo del cuadro de diálogo **Agregar scaffold,** seleccione**Agregar** **identidad** > .</span><span class="sxs-lookup"><span data-stu-id="acf53-131">From the left pane of the **Add Scaffold** dialog, select **Identity** > **Add**.</span></span>
* <span data-ttu-id="acf53-132">En el cuadro de diálogo **Agregar identidad,** las siguientes opciones:</span><span class="sxs-lookup"><span data-stu-id="acf53-132">In the **Add Identity** dialog, the following options:</span></span>
  * <span data-ttu-id="acf53-133">Seleccione el archivo de diseño existente *./Pages/Shared/_Layout.cshtml*</span><span class="sxs-lookup"><span data-stu-id="acf53-133">Select the existing layout  file  *~/Pages/Shared/_Layout.cshtml*</span></span>
  * <span data-ttu-id="acf53-134">Seleccione los siguientes archivos que desea anular:</span><span class="sxs-lookup"><span data-stu-id="acf53-134">Select the following files to override:</span></span>
    * <span data-ttu-id="acf53-135">**Cuenta/Registro**</span><span class="sxs-lookup"><span data-stu-id="acf53-135">**Account/Register**</span></span>
    * <span data-ttu-id="acf53-136">**Cuenta/Administrar/Índice**</span><span class="sxs-lookup"><span data-stu-id="acf53-136">**Account/Manage/Index**</span></span>
  * <span data-ttu-id="acf53-137">Seleccione **+** el botón para crear una nueva clase de **contexto de**datos.</span><span class="sxs-lookup"><span data-stu-id="acf53-137">Select the **+** button to create a new **Data context class**.</span></span> <span data-ttu-id="acf53-138">Acepte el tipo (**WebApp1.Models.WebApp1Context** si el proyecto se denomina **WebApp1**).</span><span class="sxs-lookup"><span data-stu-id="acf53-138">Accept the type (**WebApp1.Models.WebApp1Context** if the project is named **WebApp1**).</span></span>
  * <span data-ttu-id="acf53-139">Seleccione **+** el botón para crear una nueva **clase User**.</span><span class="sxs-lookup"><span data-stu-id="acf53-139">Select the **+** button to create a new **User class**.</span></span> <span data-ttu-id="acf53-140">Acepte el tipo (**WebApp1User** si el proyecto se denomina **WebApp1**) > **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="acf53-140">Accept the type (**WebApp1User** if the project is named **WebApp1**) > **Add**.</span></span>
* <span data-ttu-id="acf53-141">Seleccione **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="acf53-141">Select **Add**.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="acf53-142">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="acf53-142">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="acf53-143">Si no ha instalado previamente el scaffolder ASP.NET Core, instálelo ahora:</span><span class="sxs-lookup"><span data-stu-id="acf53-143">If you have not previously installed the ASP.NET Core scaffolder, install it now:</span></span>

```dotnetcli
dotnet tool install -g dotnet-aspnet-codegenerator
```

<span data-ttu-id="acf53-144">Agregue una referencia de paquete a [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) al archivo de proyecto (.csproj).</span><span class="sxs-lookup"><span data-stu-id="acf53-144">Add a package reference to [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) to the project (.csproj) file.</span></span> <span data-ttu-id="acf53-145">Ejecute el siguiente comando en el directorio del proyecto:</span><span class="sxs-lookup"><span data-stu-id="acf53-145">Run the following command in the project directory:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

<span data-ttu-id="acf53-146">Ejecute el siguiente comando para enumerar las opciones de Scaffolder identity:</span><span class="sxs-lookup"><span data-stu-id="acf53-146">Run the following command to list the Identity scaffolder options:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity -h
```

<span data-ttu-id="acf53-147">En la carpeta del proyecto, ejecute el scaffolder Identity:</span><span class="sxs-lookup"><span data-stu-id="acf53-147">In the project folder, run the Identity scaffolder:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity -u WebApp1User -fi Account.Register;Account.Manage.Index
```

---

<span data-ttu-id="acf53-148">Siga las instrucciones de [Migraciones, UseAuthentication y diseño](xref:security/authentication/scaffold-identity#efm) para realizar los pasos siguientes:</span><span class="sxs-lookup"><span data-stu-id="acf53-148">Follow the instruction in [Migrations, UseAuthentication, and layout](xref:security/authentication/scaffold-identity#efm) to perform the following steps:</span></span>

* <span data-ttu-id="acf53-149">Cree una migración y actualice la base de datos.</span><span class="sxs-lookup"><span data-stu-id="acf53-149">Create a migration and update the database.</span></span>
* <span data-ttu-id="acf53-150">Agregue `UseAuthentication` a `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="acf53-150">Add `UseAuthentication` to `Startup.Configure`.</span></span>
* <span data-ttu-id="acf53-151">Agregar `<partial name="_LoginPartial" />` al archivo de diseño.</span><span class="sxs-lookup"><span data-stu-id="acf53-151">Add `<partial name="_LoginPartial" />` to the layout file.</span></span>
* <span data-ttu-id="acf53-152">Pruebe la aplicación:</span><span class="sxs-lookup"><span data-stu-id="acf53-152">Test the app:</span></span>
  * <span data-ttu-id="acf53-153">Registrar un usuario</span><span class="sxs-lookup"><span data-stu-id="acf53-153">Register a user</span></span>
  * <span data-ttu-id="acf53-154">Seleccione el nuevo nombre de usuario (junto al vínculo **Cerrar sesión).**</span><span class="sxs-lookup"><span data-stu-id="acf53-154">Select the new user name (next to the **Logout** link).</span></span> <span data-ttu-id="acf53-155">Es posible que deba expandir la ventana o seleccionar el icono de la barra de navegación para mostrar el nombre de usuario y otros vínculos.</span><span class="sxs-lookup"><span data-stu-id="acf53-155">You might need to expand the window or select the navigation bar icon to show the user name and other links.</span></span>
  * <span data-ttu-id="acf53-156">Seleccione la pestaña **Datos personales.**</span><span class="sxs-lookup"><span data-stu-id="acf53-156">Select the **Personal Data** tab.</span></span>
  * <span data-ttu-id="acf53-157">Seleccione el botón **Descargar** y examine el archivo *PersonalData.json.*</span><span class="sxs-lookup"><span data-stu-id="acf53-157">Select the **Download** button and examined the *PersonalData.json* file.</span></span>
  * <span data-ttu-id="acf53-158">Pruebe el botón **Eliminar,** que elimina el usuario que ha iniciado sesión.</span><span class="sxs-lookup"><span data-stu-id="acf53-158">Test the **Delete** button, which deletes the logged on user.</span></span>

## <a name="add-custom-user-data-to-the-identity-db"></a><span data-ttu-id="acf53-159">Agregue datos de usuario personalizados a la base de datos de identidades</span><span class="sxs-lookup"><span data-stu-id="acf53-159">Add custom user data to the Identity DB</span></span>

<span data-ttu-id="acf53-160">Actualice `IdentityUser` la clase derivada con propiedades personalizadas.</span><span class="sxs-lookup"><span data-stu-id="acf53-160">Update the `IdentityUser` derived class with custom properties.</span></span> <span data-ttu-id="acf53-161">Si ha denominado el proyecto WebApp1, el archivo se denomina *Areas/Identity/Data/WebApp1User.cs*.</span><span class="sxs-lookup"><span data-stu-id="acf53-161">If you named the project WebApp1, the file is named *Areas/Identity/Data/WebApp1User.cs*.</span></span> <span data-ttu-id="acf53-162">Actualice el archivo con el siguiente código:</span><span class="sxs-lookup"><span data-stu-id="acf53-162">Update the file with the following code:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Data/WebApp1User.cs)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Data/WebApp1User.cs)]

::: moniker-end

<span data-ttu-id="acf53-163">Las propiedades con el atributo [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute) son:</span><span class="sxs-lookup"><span data-stu-id="acf53-163">Properties with the [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute) attribute are:</span></span>

* <span data-ttu-id="acf53-164">Se elimina cuando la página de Razor *Areas/Identity/Pages/Account/Manage/DeletePersonalData.cshtml* llama `UserManager.Delete`a la página .</span><span class="sxs-lookup"><span data-stu-id="acf53-164">Deleted when the *Areas/Identity/Pages/Account/Manage/DeletePersonalData.cshtml* Razor Page calls `UserManager.Delete`.</span></span>
* <span data-ttu-id="acf53-165">Incluido en los datos descargados por el *Areas/Identity/Pages/Account/Manage/DownloadPersonalData.cshtml* Razor Page.</span><span class="sxs-lookup"><span data-stu-id="acf53-165">Included in the downloaded data by the *Areas/Identity/Pages/Account/Manage/DownloadPersonalData.cshtml* Razor Page.</span></span>

### <a name="update-the-accountmanageindexcshtml-page"></a><span data-ttu-id="acf53-166">Actualizar la página Account/Manage/Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="acf53-166">Update the Account/Manage/Index.cshtml page</span></span>

<span data-ttu-id="acf53-167">Actualice `InputModel` el en *Areas/Identity/Pages/Account/Manage/Index.cshtml.cs* con el siguiente código resaltado:</span><span class="sxs-lookup"><span data-stu-id="acf53-167">Update the `InputModel` in *Areas/Identity/Pages/Account/Manage/Index.cshtml.cs* with the following highlighted code:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml.cs?name=snippet&highlight=24-32,48-49,96-104,106)]

<span data-ttu-id="acf53-168">Actualice *areas/Identity/Pages/Account/Manage/Index.cshtml* con el siguiente marcado resaltado:</span><span class="sxs-lookup"><span data-stu-id="acf53-168">Update the *Areas/Identity/Pages/Account/Manage/Index.cshtml* with the following highlighted markup:</span></span>

[!code-cshtml[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml?highlight=18-25)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml.cs?name=snippet&highlight=28-36,63-64,98-106,119)]

<span data-ttu-id="acf53-169">Actualice *areas/Identity/Pages/Account/Manage/Index.cshtml* con el siguiente marcado resaltado:</span><span class="sxs-lookup"><span data-stu-id="acf53-169">Update the *Areas/Identity/Pages/Account/Manage/Index.cshtml* with the following highlighted markup:</span></span>

[!code-cshtml[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml?highlight=35-42)]

::: moniker-end

### <a name="update-the-accountregistercshtml-page"></a><span data-ttu-id="acf53-170">Actualizar la página Account/Register.cshtml</span><span class="sxs-lookup"><span data-stu-id="acf53-170">Update the Account/Register.cshtml page</span></span>

<span data-ttu-id="acf53-171">Actualice `InputModel` el en *Areas/Identity/Pages/Account/Register.cshtml.cs* con el siguiente código resaltado:</span><span class="sxs-lookup"><span data-stu-id="acf53-171">Update the `InputModel` in *Areas/Identity/Pages/Account/Register.cshtml.cs* with the following highlighted code:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=30-38,70-71)]

<span data-ttu-id="acf53-172">Actualice *areas/Identity/Pages/Account/Register.cshtml* con el siguiente marcado resaltado:</span><span class="sxs-lookup"><span data-stu-id="acf53-172">Update the *Areas/Identity/Pages/Account/Register.cshtml* with the following highlighted markup:</span></span>

[!code-cshtml[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml?highlight=16-25)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=28-36,67,66)]

<span data-ttu-id="acf53-173">Actualice *areas/Identity/Pages/Account/Register.cshtml* con el siguiente marcado resaltado:</span><span class="sxs-lookup"><span data-stu-id="acf53-173">Update the *Areas/Identity/Pages/Account/Register.cshtml* with the following highlighted markup:</span></span>

[!code-cshtml[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml?highlight=16-25)]

::: moniker-end


<span data-ttu-id="acf53-174">Compile el proyecto.</span><span class="sxs-lookup"><span data-stu-id="acf53-174">Build the project.</span></span>

### <a name="add-a-migration-for-the-custom-user-data"></a><span data-ttu-id="acf53-175">Agregar una migración para los datos de usuario personalizados</span><span class="sxs-lookup"><span data-stu-id="acf53-175">Add a migration for the custom user data</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="acf53-176">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="acf53-176">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="acf53-177">En la consola del **Administrador de paquetes**de Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="acf53-177">In the Visual Studio **Package Manager Console**:</span></span>

```powershell
Add-Migration CustomUserData
Update-Database
```

# <a name="net-core-cli"></a>[<span data-ttu-id="acf53-178">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="acf53-178">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet ef migrations add CustomUserData
dotnet ef database update
```

---

## <a name="test-create-view-download-delete-custom-user-data"></a><span data-ttu-id="acf53-179">Probar crear, ver, descargar, eliminar datos de usuario personalizados</span><span class="sxs-lookup"><span data-stu-id="acf53-179">Test create, view, download, delete custom user data</span></span>

<span data-ttu-id="acf53-180">Pruebe la aplicación:</span><span class="sxs-lookup"><span data-stu-id="acf53-180">Test the app:</span></span>

* <span data-ttu-id="acf53-181">Registre un nuevo usuario.</span><span class="sxs-lookup"><span data-stu-id="acf53-181">Register a new user.</span></span>
* <span data-ttu-id="acf53-182">Vea los datos de `/Identity/Account/Manage` usuario personalizados en la página.</span><span class="sxs-lookup"><span data-stu-id="acf53-182">View the custom user data on the `/Identity/Account/Manage` page.</span></span>
* <span data-ttu-id="acf53-183">Descargue y vea los datos `/Identity/Account/Manage/PersonalData` personales de los usuarios desde la página.</span><span class="sxs-lookup"><span data-stu-id="acf53-183">Download and view the users personal data from the `/Identity/Account/Manage/PersonalData` page.</span></span>

## <a name="add-claims-to-identity-using-iuserclaimsprincipalfactoryapplicationuser"></a><span data-ttu-id="acf53-184">Agregar notificaciones a Identity mediante IUserClaimsPrincipalFactory<ApplicationUser></span><span class="sxs-lookup"><span data-stu-id="acf53-184">Add claims to Identity using IUserClaimsPrincipalFactory<ApplicationUser></span></span>

<span data-ttu-id="acf53-185">Se pueden agregar notificaciones adicionales a `IUserClaimsPrincipalFactory<T>` ASP.NET identidad principal mediante la interfaz.</span><span class="sxs-lookup"><span data-stu-id="acf53-185">Additional claims can be added to ASP.NET Core Identity by using the `IUserClaimsPrincipalFactory<T>` interface.</span></span> <span data-ttu-id="acf53-186">Esta clase se puede agregar `Startup.ConfigureServices` a la aplicación en el método.</span><span class="sxs-lookup"><span data-stu-id="acf53-186">This class can be added to the app in the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="acf53-187">Agregue la implementación personalizada de la clase de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="acf53-187">Add the custom implementation of the class as follows:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

    services.AddScoped<IUserClaimsPrincipalFactory<ApplicationUser>, 
        AdditionalUserClaimsPrincipalFactory>();
```

<span data-ttu-id="acf53-188">El código de `ApplicationUser` demostración utiliza la clase.</span><span class="sxs-lookup"><span data-stu-id="acf53-188">The demo code uses the `ApplicationUser` class.</span></span> <span data-ttu-id="acf53-189">Esta clase agrega `IsAdmin` una propiedad que se utiliza para agregar la notificación adicional.</span><span class="sxs-lookup"><span data-stu-id="acf53-189">This class adds an `IsAdmin` property which is used to add the additional claim.</span></span>

```csharp
public class ApplicationUser : IdentityUser
{
    public bool IsAdmin { get; set; }
}
```

<span data-ttu-id="acf53-190">`AdditionalUserClaimsPrincipalFactory` implementa la interfaz `UserClaimsPrincipalFactory`.</span><span class="sxs-lookup"><span data-stu-id="acf53-190">The `AdditionalUserClaimsPrincipalFactory` implements the `UserClaimsPrincipalFactory` interface.</span></span> <span data-ttu-id="acf53-191">Se agrega una nueva `ClaimsPrincipal`notificación de rol al archivo .</span><span class="sxs-lookup"><span data-stu-id="acf53-191">A new role claim is added to the `ClaimsPrincipal`.</span></span>

```csharp
public class AdditionalUserClaimsPrincipalFactory 
        : UserClaimsPrincipalFactory<ApplicationUser, IdentityRole>
{
    public AdditionalUserClaimsPrincipalFactory( 
        UserManager<ApplicationUser> userManager,
        RoleManager<IdentityRole> roleManager, 
        IOptions<IdentityOptions> optionsAccessor) 
        : base(userManager, roleManager, optionsAccessor)
    {}

    public async override Task<ClaimsPrincipal> CreateAsync(ApplicationUser user)
    {
        var principal = await base.CreateAsync(user);
        var identity = (ClaimsIdentity)principal.Identity;

        var claims = new List<Claim>();
        if (user.IsAdmin)
        {
            claims.Add(new Claim(JwtClaimTypes.Role, "admin"));
        }
        else
        {
            claims.Add(new Claim(JwtClaimTypes.Role, "user"));
        }

        identity.AddClaims(claims);
        return principal;
    }
}
```

<span data-ttu-id="acf53-192">La notificación adicional se puede usar en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="acf53-192">The additional claim can then be used in the app.</span></span> <span data-ttu-id="acf53-193">En una página `IAuthorizationService` de Razor, la instancia se puede usar para tener acceso al valor de notificación.</span><span class="sxs-lookup"><span data-stu-id="acf53-193">In a Razor Page, the `IAuthorizationService` instance can be used to access the claim value.</span></span>

```cshtml
@using Microsoft.AspNetCore.Authorization
@inject IAuthorizationService AuthorizationService

@if ((await AuthorizationService.AuthorizeAsync(User, "IsAdmin")).Succeeded)
{
    <ul class="mr-auto navbar-nav">
        <li class="nav-item">
            <a class="nav-link" asp-controller="Admin" asp-action="Index">ADMIN</a>
        </li>
    </ul>
}
```
