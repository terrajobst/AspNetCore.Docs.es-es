---
title: Agregar, descargar y eliminar datos de usuario a la identidad en un proyecto de ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo agregar datos de usuario personalizada para la identidad en un proyecto de ASP.NET Core. Eliminar datos de RGPD.
ms.author: riande
ms.date: 6/16/2018
ms.custom: seodec18
uid: security/authentication/add-user-data
ms.openlocfilehash: 8c0413a16d92b717619387748ee78f0d14d6c852
ms.sourcegitcommit: 9b7fcb4ce00a3a32e153a080ebfaae4ef417aafa
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/12/2019
ms.locfileid: "59516214"
---
# <a name="add-download-and-delete-custom-user-data-to-identity-in-an-aspnet-core-project"></a>Agregar, descargar y eliminar datos de usuario personalizado a la identidad en un proyecto de ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

Este artículo se muestra cómo:

* Agregar datos de usuario personalizado a una aplicación web ASP.NET Core.
* Incorpore al modelo de datos de usuario personalizado con el [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute?view=aspnetcore-2.1) atributo para que esté disponible automáticamente para la descarga y eliminación. Hacer que los datos que puede descargarse y eliminado ayuda a cumplir [RGPD](xref:security/gdpr) requisitos.

Se crea el proyecto de ejemplo desde una aplicación web de páginas de Razor, pero las instrucciones son similares para una aplicación web de ASP.NET Core MVC.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/authentication/add-user-data) ([cómo descargarlo](xref:index#how-to-download-a-sample))

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE [](~/includes/2.2-SDK.md)]

## <a name="create-a-razor-web-app"></a>Creación de una aplicación web de Razor

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* En el menú **Archivo** de Visual Studio, seleccione **Nuevo** > **Proyecto**. Denomine el proyecto **WebApp1** si desea coincida con el espacio de nombres de los [Descargar ejemplo](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/authentication/add-user-data/sample) código.
* Seleccione **aplicación Web ASP.NET Core** > **Aceptar**
* Seleccione **2.2 de ASP.NET Core** en la lista desplegable
* Seleccione **aplicación Web**  > **Aceptar**
* Compile y ejecute el proyecto.

# <a name="net-core-clitabnetcore-cli"></a>[CLI de .NET Core](#tab/netcore-cli)

```cli
dotnet new webapp -o WebApp1
```

---

## <a name="run-the-identity-scaffolder"></a>Ejecute el proveedor de scaffolding de identidad

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Desde **el Explorador de soluciones**, haga doble clic en el proyecto > **agregar** > **nuevo elemento de scaffolding**.
* En el panel izquierdo de la **agregar Scaffold** cuadro de diálogo, seleccione **identidad** > **agregar**.
* En el **identidad de ADD** cuadro de diálogo, las siguientes opciones:
  * Seleccione el archivo de diseño existente *~/Pages/Shared/_Layout.cshtml*
  * Seleccione los archivos siguientes para reemplazar:
    * **Cuenta/Register**
    * **Cuenta/administrar o índice**
  * Seleccione el **+** botón para crear un nuevo **clase de contexto de datos**. Acepte el tipo (**WebApp1.Models.WebApp1Context** si el proyecto se denomina **WebApp1**).
  * Seleccione el **+** botón para crear un nuevo **clase User**. Acepte el tipo (**WebApp1User** si el proyecto se denomina **WebApp1**) > **agregar**.
* Seleccione **agregar**.

# <a name="net-core-clitabnetcore-cli"></a>[CLI de .NET Core](#tab/netcore-cli)

Si no ha instalado previamente el proveedor de scaffolding de ASP.NET Core, instalar ahora:

```cli
dotnet tool install -g dotnet-aspnet-codegenerator
```

Agregue una referencia de paquete a [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) al archivo de proyecto (.csproj). Ejecute el siguiente comando en el directorio del proyecto:

```cli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

Ejecute el siguiente comando para enumerar las opciones de proveedor de scaffolding de identidad:

```cli
dotnet aspnet-codegenerator identity -h
```

En la carpeta del proyecto, ejecute el proveedor de scaffolding de identidad:

```cli
dotnet aspnet-codegenerator identity -u WebApp1User -fi Account.Register;Account.Manage.Index
```

---

Siga las instrucciones [migraciones, UseAuthentication y diseño](xref:security/authentication/scaffold-identity#efm) para realizar los pasos siguientes:

* Cree una migración y actualización de la base de datos.
* Agregue `UseAuthentication` a `Startup.Configure`.
* Agregar `<partial name="_LoginPartial" />` para el archivo de diseño.
* Pruebe la aplicación:
  * Registrar un usuario
  * Seleccione el nuevo nombre de usuario (junto a la **Logout** vínculo). Es posible que deba expandir la ventana o seleccione el icono de la barra de navegación para mostrar el nombre de usuario y otros vínculos.
  * Seleccione el **datos personales** ficha.
  * Seleccione el **descargar** botón y examinar el *PersonalData.json* archivo.
  * Prueba la **eliminar** botón, lo que elimina el inicio de sesión de usuario.

## <a name="add-custom-user-data-to-the-identity-db"></a>Agregar datos de usuario personalizado a la base de datos de identidad

Actualización de la `IdentityUser` derivados de la clase con propiedades personalizadas. Si el nombre del proyecto WebApp1, el archivo se denomina *Areas/Identity/Data/WebApp1User.cs*. Actualice el archivo con el código siguiente:

[!code-csharp[Main](add-user-data/sample-2.2/Areas/Identity/Data/WebApp1User.cs)]

Propiedades decoran con el [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute?view=aspnetcore-2.1) atributo son:

* Cuando elimina el *Areas/Identity/Pages/Account/Manage/DeletePersonalData.cshtml* llama la página de Razor `UserManager.Delete`.
* Incluye en los datos descargados por el *Areas/Identity/Pages/Account/Manage/DownloadPersonalData.cshtml* página de Razor.

### <a name="update-the-accountmanageindexcshtml-page"></a>Actualizar la página Account/Manage/Index.cshtml

Actualización de la `InputModel` en *Areas/Identity/Pages/Account/Manage/Index.cshtml.cs* con el siguiente código resaltado:

[!code-csharp[Main](add-user-data/sample-2.2/Areas/Identity/Pages/Account/Manage/Index.cshtml.cs?name=snippet&highlight=28-36,63-64,98-106,119)]

Actualización de la *Areas/Identity/Pages/Account/Manage/Index.cshtml* con el siguiente marcado resaltado:

[!code-html[Main](add-user-data/sample-2.2/Areas/Identity/Pages/Account/Manage/Index.cshtml?highlight=35-42)]

### <a name="update-the-accountregistercshtml-page"></a>Actualizar la página Account/Register.cshtml

Actualización de la `InputModel` en *Areas/Identity/Pages/Account/Register.cshtml.cs* con el siguiente código resaltado:

[!code-csharp[Main](add-user-data/sample-2.2/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=28-36,67,66)]

Actualización de la *Areas/Identity/Pages/Account/Register.cshtml* con el siguiente marcado resaltado:

[!code-html[Main](add-user-data/sample-2.2/Areas/Identity/Pages/Account/Register.cshtml?highlight=16-25)]

Compile el proyecto.

### <a name="add-a-migration-for-the-custom-user-data"></a>Agrega una migración para los datos de usuario personalizada

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

En Visual Studio **Package Manager Console**:

```PMC
Add-Migration CustomUserData
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[CLI de .NET Core](#tab/netcore-cli)

```cli
dotnet ef migrations add CustomUserData
dotnet ef database update
```

---

## <a name="test-create-view-download-delete-custom-user-data"></a>Prueba crear, ver, descargar y eliminar datos de usuario personalizada

Pruebe la aplicación:

* Registrar un nuevo usuario.
* Ver los datos de usuario personalizada en el `/Identity/Account/Manage` página.
* Descargar y ver los datos personales de los usuarios desde el `/Identity/Account/Manage/PersonalData` página.
