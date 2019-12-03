---
title: Agregar, descargar y eliminar datos de usuario de una identidad en un proyecto de ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo agregar datos de usuario personalizados a la identidad en un proyecto de ASP.NET Core. Elimine los datos por RGPD.
ms.author: riande
ms.date: 06/18/2019
ms.custom: mvc, seodec18
uid: security/authentication/add-user-data
ms.openlocfilehash: 6daca5776930f80eec8d81132b5a5c4d4d5c13ad
ms.sourcegitcommit: 0dd224b2b7efca1fda0041b5c3f45080327033f6
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/02/2019
ms.locfileid: "74681167"
---
# <a name="add-download-and-delete-custom-user-data-to-identity-in-an-aspnet-core-project"></a>Agregar, descargar y eliminar datos de usuario personalizados en una identidad en un proyecto de ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

En este artículo se muestra cómo:

* Agregue datos de usuario personalizados a una aplicación Web de ASP.NET Core.
* Decorar el modelo de datos de usuario personalizado con el atributo <xref:Microsoft.AspNetCore.Identity.PersonalDataAttribute> para que esté disponible automáticamente para su descarga y eliminación. Hacer que los datos se puedan descargar y eliminar ayuda a cumplir los requisitos de [RGPD](xref:security/gdpr) .

El ejemplo de proyecto se crea a partir de una aplicación Web de Razor Pages, pero las instrucciones son similares para una aplicación Web de ASP.NET Core MVC.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/add-user-data) ([cómo descargarlo](xref:index#how-to-download-a-sample))

## <a name="prerequisites"></a>Requisitos previos

::: moniker range=">= aspnetcore-3.0"

[!INCLUDE [](~/includes/3.0-SDK.md)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!INCLUDE [](~/includes/2.2-SDK.md)]

::: moniker-end

## <a name="create-a-razor-web-app"></a>Crear una aplicación web de Razor

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

::: moniker range=">= aspnetcore-3.0"

* En el menú **Archivo** de Visual Studio, seleccione **Nuevo** > **Proyecto**. Asigne al proyecto el nombre **WebApp1** si desea que coincida con el espacio de nombres del código de [ejemplo de descarga](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/authentication/add-user-data) .
* Seleccione **ASP.net Core aplicación Web** > **Aceptar** .
* Seleccione **ASP.NET Core 3,0** en la lista desplegable.
* Seleccionar **aplicación Web** > **Aceptar**
* Cree y ejecute el proyecto.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* En el menú **Archivo** de Visual Studio, seleccione **Nuevo** > **Proyecto**. Asigne al proyecto el nombre **WebApp1** si desea que coincida con el espacio de nombres del código de [ejemplo de descarga](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/authentication/add-user-data) .
* Seleccione **ASP.net Core aplicación Web** > **Aceptar** .
* Seleccione **ASP.NET Core 2,2** en la lista desplegable.
* Seleccionar **aplicación Web** > **Aceptar**
* Cree y ejecute el proyecto.

::: moniker-end


# <a name="net-core-clitabnetcore-cli"></a>[CLI de .NET Core](#tab/netcore-cli)

```dotnetcli
dotnet new webapp -o WebApp1
```

---

## <a name="run-the-identity-scaffolder"></a>Ejecutar el scaffolding de identidad

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* En **Explorador de soluciones**, haga clic con el botón derecho en el proyecto > **Agregar** > **nuevo elemento con scaffolding**.
* En el panel izquierdo del cuadro de diálogo **Agregar scaffold** , seleccione **Identity** > **Agregar**.
* En el cuadro de diálogo **Agregar identidad** , las siguientes opciones:
  * Seleccione el archivo de diseño existente *~/Pages/Shared/_Layout. cshtml*
  * Seleccione los siguientes archivos para invalidar:
    * **Cuenta/registro**
    * **Cuenta/administración/índice**
  * Seleccione el botón **+** para crear una nueva **clase de contexto de datos**. Acepte el tipo (**WebApp1. Models. WebApp1Context** si el proyecto se denomina **WebApp1**).
  * Seleccione el botón **+** para crear una nueva **clase de usuario**. Acepte el tipo (**WebApp1User** si el proyecto se denomina **WebApp1**) > **Agregar**.
* Seleccione **Agregar**.

# <a name="net-core-clitabnetcore-cli"></a>[CLI de .NET Core](#tab/netcore-cli)

Si no ha instalado previamente el scaffolding de ASP.NET Core, instálelo ahora:

```dotnetcli
dotnet tool install -g dotnet-aspnet-codegenerator
```

Agregue una referencia de paquete a [Microsoft. VisualStudio. Web. CodeGeneration. Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) al archivo de proyecto (. csproj). Ejecute el siguiente comando en el directorio del proyecto:

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

Ejecute el siguiente comando para enumerar las opciones del scaffolding de identidad:

```dotnetcli
dotnet aspnet-codegenerator identity -h
```

En la carpeta del proyecto, ejecute el scaffolding de identidad:

```dotnetcli
dotnet aspnet-codegenerator identity -u WebApp1User -fi Account.Register;Account.Manage.Index
```

---

Siga las instrucciones de [migraciones, UseAuthentication y diseño](xref:security/authentication/scaffold-identity#efm) para realizar los pasos siguientes:

* Cree una migración y actualice la base de datos.
* Agregue `UseAuthentication` a `Startup.Configure`.
* Agregue `<partial name="_LoginPartial" />` al archivo de diseño.
* Pruebe la aplicación:
  * Registrar un usuario
  * Seleccione el nuevo nombre de usuario (junto al vínculo de **cierre de sesión** ). Es posible que necesite expandir la ventana o seleccionar el icono de la barra de navegación para mostrar el nombre de usuario y otros vínculos.
  * Seleccione la pestaña **datos personales** .
  * Seleccione el botón **Descargar** y examine el archivo *PersonalData. JSON* .
  * Pruebe el botón **eliminar** , que elimina el usuario que ha iniciado sesión.

## <a name="add-custom-user-data-to-the-identity-db"></a>Agregar datos de usuario personalizados a la base de datos de identidad

Actualice la clase derivada `IdentityUser` con propiedades personalizadas. Si ha llamado al proyecto WebApp1, el archivo se denomina *areas/Identity/Data/WebApp1User. CS*. Actualice el archivo con el código siguiente:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Data/WebApp1User.cs)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Data/WebApp1User.cs)]

::: moniker-end

Las propiedades decoradas con el atributo [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute) son:

* Se elimina cuando la página de Razor *areas/Identity/pages/Account/Manage/DeletePersonalData. cshtml* llama a `UserManager.Delete`.
* Se incluye en los datos descargados mediante la página de Razor *areas/Identity/pages/Account/Manage/DownloadPersonalData. cshtml* .

### <a name="update-the-accountmanageindexcshtml-page"></a>Actualización de la página Account/Manage/index. cshtml

Actualice el `InputModel` en *areas/Identity/pages/Account/Manage/index. cshtml. CS* con el siguiente código resaltado:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml.cs?name=snippet&highlight=24-32,48-49,96-104,106)]

Actualice las *áreas/Identity/pages/Account/Manage/index. cshtml* con el siguiente marcado resaltado:

[!code-cshtml[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml?highlight=18-25)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml.cs?name=snippet&highlight=28-36,63-64,98-106,119)]

Actualice las *áreas/Identity/pages/Account/Manage/index. cshtml* con el siguiente marcado resaltado:

[!code-chtml[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml?highlight=35-42)]

::: moniker-end

### <a name="update-the-accountregistercshtml-page"></a>Actualización de la página cuenta/registro. cshtml

Actualice el `InputModel` en *areas/Identity/pages/Account/Register. cshtml. CS* con el siguiente código resaltado:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=30-38,70-71)]

Actualice las *áreas/Identity/pages/Account/Register. cshtml* con el siguiente marcado resaltado:

[!code-cshtml[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml?highlight=16-25)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=28-36,67,66)]

Actualice las *áreas/Identity/pages/Account/Register. cshtml* con el siguiente marcado resaltado:

[!code-chtml[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml?highlight=16-25)]

::: moniker-end


Generar el proyecto.

### <a name="add-a-migration-for-the-custom-user-data"></a>Agregar una migración para los datos de usuario personalizados

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

En la consola del **Administrador de paquetes**de Visual Studio:

```powershell
Add-Migration CustomUserData
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[CLI de .NET Core](#tab/netcore-cli)

```dotnetcli
dotnet ef migrations add CustomUserData
dotnet ef database update
```

---

## <a name="test-create-view-download-delete-custom-user-data"></a>Prueba crear, ver, descargar y eliminar datos de usuario personalizados

Pruebe la aplicación:

* Registra un nuevo usuario.
* Vea los datos de usuario personalizados en la página `/Identity/Account/Manage`.
* Descargue y vea los datos personales de los usuarios en la página `/Identity/Account/Manage/PersonalData`.
