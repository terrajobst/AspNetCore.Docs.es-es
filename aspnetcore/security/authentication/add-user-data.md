---
title: Agregar, descargar y eliminar datos de usuario personalizada para la identidad en un proyecto de ASP.NET Core
author: rick-anderson
description: Obtenga información acerca de cómo agregar datos de usuario personalizada para la identidad en un proyecto de ASP.NET Core. Eliminar datos por GDPR.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 6/16/2018
uid: security/authentication/add-user-data
ms.openlocfilehash: ecd0e6d1c71b24309fab70fbb06af7731463bb0e
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2018
ms.locfileid: "36271962"
---
# <a name="add-download-and-delete-custom-user-data-to-identity-in-an-aspnet-core-project"></a>Agregar, descargar y eliminar datos de usuario personalizada para la identidad en un proyecto de ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

Este artículo se muestra cómo:

* Agregar datos de usuario personalizado a una aplicación web de ASP.NET Core.
* Decorar el modelo de datos de usuario personalizada con el [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute?view=aspnetcore-2.1) atributo para que esté disponible automáticamente para su eliminación y la descarga. Hacer que los datos que se puede descargar y eliminar ayuda a cumplir [GDPR](xref:security/gdpr) requisitos.

Se crea el proyecto de ejemplo desde una aplicación web de las páginas de Razor, pero las instrucciones son similares para una aplicación web de MVC de ASP.NET Core.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/authentication/add-user-data/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE [](~/includes/2.1-SDK.md)]

## <a name="create-a-razor-web-app"></a>Creación de una aplicación web de Razor

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* En el menú **Archivo** de Visual Studio, seleccione **Nuevo** > **Proyecto**. Denomine el proyecto **WebApp1** si desea coincida con el espacio de nombres de la [Descargar ejemplo](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/authentication/add-user-data/sample) código.
* Seleccione **aplicación Web de ASP.NET Core** > **Aceptar**
* Seleccione **ASP.NET Core 2.1** en la lista desplegable
* Seleccione **aplicación Web**  > **Aceptar**
* Compile y ejecute el proyecto.

# <a name="net-core-clitabnetcore-cli"></a>[CLI de .NET Core](#tab/netcore-cli)

```cli
dotnet new webapp -o WebApp1
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

---

## <a name="run-the-identity-scaffolder"></a>Ejecute al scaffolder de identidad

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* De **el Explorador de soluciones**, haga doble clic en el proyecto > **agregar** > **nuevo elemento de scaffolding**.
* En el panel izquierdo de la **agregar scaffolding** cuadro de diálogo, seleccione **identidad** > **agregar**.
* En el **Agregar identidad** cuadro de diálogo, las siguientes opciones:
  * Seleccione el archivo de diseño existente *~/Pages/Shared/_Layout.cshtml*
  * Seleccione los archivos siguientes para reemplazar:
    * **Cuenta/Register**
    * **Cuenta/Administrar/índice**
  * Seleccione el **+** botón para crear un nuevo **clase de contexto de datos**. Acepte el tipo (**WebApp1.Models.WebApp1Context** si denominado el proyecto **WebApp1**).
  * Seleccione el **+** botón para crear un nuevo **User (clase)**. Acepte el tipo (**WebApp1User** si denominado el proyecto **WebApp1**) > **agregar**.
* Seleccione **agregar**.

# <a name="net-core-clitabnetcore-cli"></a>[CLI de .NET Core](#tab/netcore-cli)

Si no ha instalado previamente el scaffolder ASP.NET, instalarla ahora:

```cli
dotnet tool install -g dotnet-aspnet-codegenerator
```

Agregue una referencia de paquete a [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) al archivo de proyecto (.csproj). Ejecute el siguiente comando en el directorio del proyecto:

```cli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

Ejecute el siguiente comando para mostrar las opciones de scaffolder de identidad:

```cli
dotnet aspnet-codegenerator identity -h
```

En la carpeta del proyecto, ejecute al scaffolder de identidad:

```cli
dotnet aspnet-codegenerator identity -u WebApp1User -fi Account.Register;Account.Manage.Index
```

-------------

Siga las instrucciones [UseAuthentication, migraciones y diseño](xref:security/authentication/scaffold-identity#efm) para realizar los pasos siguientes:

* Cree una migración y actualice la base de datos.
* Agregue `UseAuthentication` a `Startup.Configure`.
* Agregar `<partial name="_LoginPartial" />` para el archivo de diseño.
* Pruebe la aplicación:
  * Registrar un usuario
  * Seleccione el nuevo nombre de usuario (junto a la **Logout** vínculo). Deberá expandir la ventana o seleccione el icono de la barra de navegación para mostrar el nombre de usuario y otros vínculos.
  * Seleccione el **datos personales** ficha.
  * Seleccione el **descargar** botón y examinar el *PersonalData.json* archivo.
  * Prueba la **eliminar** botón, lo que elimina el inicio de sesión de usuario.

## <a name="add-custom-user-data-to-the-identity-db"></a>Agregar datos de usuario personalizado a la base de datos de identidad

Actualización de la `IdentityUser` deriva la clase con propiedades personalizadas. Si con el nombre de su proyecto WebApp1, el archivo se denomina *Areas/Identity/Data/WebApp1User.cs*. Actualice el archivo con el código siguiente:

[!code-csharp[Main](add-user-data/sample/Areas/Identity/Data/WebApp1User.cs)]

Propiedades decorada con el [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute?view=aspnetcore-2.1) atributo son:

* Cuando elimina el *Areas/Identity/Pages/Account/Manage/DeletePersonalData.cshtml* Razor página llama `UserManager.Delete`.
* Incluido en los datos descargados por el *Areas/Identity/Pages/Account/Manage/DownloadPersonalData.cshtml* página Razor.

### <a name="update-the-accountmanageindexcshtml-page"></a>Actualizar la página Account/Manage/Index.cshtml

Actualización de la `InputModel` en *Areas/Identity/Pages/Account/Manage/Index.cshtml.cs* aparece resaltado este código con lo siguiente:

[!code-csharp[Main](add-user-data/sample/Areas/Identity/Pages/Account/Manage/Index.cshtml.cs?name=snippet&highlight=28-36,63-64,87-95,120)]

Actualización de la *Areas/Identity/Pages/Account/Manage/Index.cshtml* con el siguiente marcado resaltado:

[!code-html[Main](add-user-data/sample/Areas/Identity/Pages/Account/Manage/Index.cshtml?highlight=34-41)]

### <a name="update-the-accountregistercshtml-page"></a>Actualizar la página Account/Register.cshtml

Actualización de la `InputModel` en *Areas/Identity/Pages/Account/Register.cshtml.cs* aparece resaltado este código con lo siguiente:

[!code-csharp[Main](add-user-data/sample/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=8-16,43,44)]

Actualización de la *Areas/Identity/Pages/Account/Register.cshtml* con el siguiente marcado resaltado:

[!code-html[Main](add-user-data/sample/Areas/Identity/Pages/Account/Register.cshtml?highlight=16-25)]

Compile el proyecto.

### <a name="add-a-migration-for-the-custom-user-data"></a>Agregar una migración de los datos de usuario personalizada

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

En Visual Studio **consola de administrador de paquetes**:

```PMC
Add-Migration CustomUserData
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[CLI de .NET Core](#tab/netcore-cli)

```cli
dotnet ef migrations add CustomUserData
dotnet ef database update
```

------

## <a name="test-create-view-download-delete-custom-user-data"></a>Prueba de crear, ver, descargar, eliminar datos de usuario personalizada

Pruebe la aplicación:

* Registrar un nuevo usuario.
* Ver los datos de usuario personalizada en el `/Identity/Account/Manage` página.
* Descargar y ver los datos personales de los usuarios de la `/Identity/Account/Manage/PersonalData` página.
