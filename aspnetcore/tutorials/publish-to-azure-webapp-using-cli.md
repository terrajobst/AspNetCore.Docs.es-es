---
title: Publicar una aplicación ASP.NET Core en Azure con las herramientas de línea de comandos
author: camsoper
description: Obtenga información sobre cómo publicar una aplicación de ASP.NET Core en Azure App Service con el cliente de línea de comandos de Git.
manager: wpickett
ms.author: casoper
ms.custom: mvc
ms.date: 11/03/2017
ms.devlang: dotnet
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
services: multiple
uid: tutorials/publish-to-azure-webapp-using-cli
ms.openlocfilehash: 3fc068096a4b8696340787aa15120a2f97d10164
ms.sourcegitcommit: 63fb07fb3f71b32daf2c9466e132f2e7cc617163
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/10/2018
ms.locfileid: "35252443"
---
# <a name="publish-an-aspnet-core-app-to-azure-with-command-line-tools"></a>Publicar una aplicación ASP.NET Core en Azure con las herramientas de línea de comandos

Por [Cam Soper](https://twitter.com/camsoper)

[!INCLUDE [Azure App Service Preview Notice](../includes/azure-apps-preview-notice.md)]

En este tutorial se explica cómo compilar e implementar una aplicación de ASP.NET Core en Microsoft Azure App Service con las herramientas de línea de comandos. Cuando termine, tendrá una aplicación web de Razor Pages integrada en ASP.NET Core y hospedada como una aplicación web de Azure App Service. Este tutorial se escribe con herramientas de línea de comandos de Windows, pero también puede aplicarse a entornos de macOS y Linux.

En este tutorial aprenderá a:

> [!div class="checklist"]
> * Crear un sitio web en Azure App Service con la CLI de Azure
> * Implementación de una aplicación de ASP.NET Core en Azure App Service con la herramienta de línea de comandos de Git

## <a name="prerequisites"></a>Requisitos previos

Para completar este tutorial, necesita:

* Una [suscripción de Microsoft Azure](https://azure.microsoft.com/free/)
* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]
* Un cliente de línea de comandos de [Git](https://www.git-scm.com/)

## <a name="create-a-web-app"></a>Creación de una aplicación web

Cree un directorio para la aplicación web, cree una aplicación de Razor Pages de ASP.NET Core y, luego, ejecute el sitio web de forma local.

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

::: moniker range=">= aspnetcore-2.1"

```console
REM Create a new ASP.NET Core Razor Pages app
dotnet new webapp -o MyApplication

REM Change to the new directory that was just created
cd MyApplication

REM Run the app
dotnet run
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
REM Create a new ASP.NET Core Razor Pages app
dotnet new razor -o MyApplication

REM Change to the new directory that was just created
cd MyApplication

REM Run the app
dotnet run
```

::: moniker-end

# <a name="othertabother"></a>[Otros problemas](#tab/other)

::: moniker range=">= aspnetcore-2.1"

```bash
# Create a new ASP.NET Core Razor Pages app
dotnet new webapp -o MyApplication

# Change to the new directory that was just created
cd MyApplication

# Run the app
dotnet run
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```bash
# Create a new ASP.NET Core Razor Pages app
dotnet new razor -o MyApplication

# Change to the new directory that was just created
cd MyApplication

# Run the app
dotnet run
```

::: moniker-end

---

![Salida de la línea de comandos](publish-to-azure-webapp-using-cli/_static/new_prj.png)

Navegue a `http://localhost:5000` para probar la aplicación.

![Sitio web en ejecución en el entorno local](publish-to-azure-webapp-using-cli/_static/app_test.png)

## <a name="create-the-azure-app-service-instance"></a>Creación de la instancia de Azure App Service

Con [Azure Cloud Shell](/azure/cloud-shell/quickstart), cree un grupo de recursos, un plan de App Service y una aplicación web de App Service.

```azurecli-interactive
# Generate a unique Web App name
let randomNum=$RANDOM*$RANDOM
webappname=tutorialApp$randomNum

# Create the DotNetAzureTutorial resource group
az group create --name DotNetAzureTutorial --location EastUS

# Create an App Service plan.
az appservice plan create --name $webappname --resource-group DotNetAzureTutorial --sku FREE

# Create the Web App
az webapp create --name $webappname --resource-group DotNetAzureTutorial --plan $webappname
```

Antes de la implementación, defina las credenciales de implementación a nivel de cuenta con el siguiente comando:

```azurecli-interactive
az webapp deployment user set --user-name <desired user name> --password <desired password>
```

Se necesita una dirección URL de implementación para implementar la aplicación con Git. Recupere una dirección URL como esta.

```azurecli-interactive
az webapp deployment source config-local-git -n $webappname -g DotNetAzureTutorial --query [url] -o tsv
```

Anote la dirección URL mostrada que termina en `.git`. Se utiliza en el paso siguiente.

## <a name="deploy-the-app-using-git"></a>Implementación de la aplicación con Git

Está listo para implementar desde el equipo local mediante Git.

> [!NOTE]
> Es seguro pasar por alto las advertencias de Git sobre los finales de línea.

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

```cmd
REM Initialize the local Git repository
git init

REM Add the contents of the working directory to the repo
git add --all

REM Commit the changes to the local repo
git commit -a -m "Initial commit"

REM Add the URL as a Git remote repository
git remote add azure <THE GIT URL YOU NOTED EARLIER>

REM Push the local repository to the remote
git push azure master
```

# <a name="othertabother"></a>[Otros problemas](#tab/other)

```bash
# Initialize the local Git repository
git init

# Add the contents of the working directory to the repo
git add --all

# Commit the changes to the local repo
git commit -a -m "Initial commit"

# Add the URL as a Git remote repository
git remote add azure <THE GIT URL YOU NOTED EARLIER>

# Push the local repository to the remote
git push azure master
```

---

Git solicitará las credenciales de implementación establecidas anteriormente. Tras la autenticación, la aplicación se inserta en la ubicación remota, se compila y se implementa.

![Resultado de la implementación de Git](publish-to-azure-webapp-using-cli/_static/post_deploy.png)

## <a name="test-the-app"></a>Prueba de la aplicación

Navegue a `https://<web app name>.azurewebsites.net` para probar la aplicación. Para mostrar la dirección en Cloud Shell o en la CLI de Azure, se usa lo siguiente:

```azurecli-interactive
az webapp show -n $webappname -g DotNetAzureTutorial --query defaultHostName -o tsv
```

![La aplicación en ejecución en Azure](publish-to-azure-webapp-using-cli/_static/app_deployed.png)

## <a name="clean-up"></a>Limpieza

Cuando termine de probar la aplicación y de inspeccionar el código y los recursos, elimine la aplicación web y el plan con la eliminación del grupo de recursos.

```azurecli-interactive
az group delete -n DotNetAzureTutorial
```

## <a name="next-steps"></a>Pasos siguientes

En este tutorial ha aprendido a:

> [!div class="checklist"]
> * Crear un sitio web en Azure App Service con la CLI de Azure
> * Implementación de una aplicación de ASP.NET Core en Azure App Service con la herramienta de línea de comandos de Git

A continuación, aprenda a usar la línea de comandos para implementar una aplicación web existente que usa Cosmos DB.

> [!div class="nextstepaction"]
> [Implementación en Azure desde la línea de comandos con .NET Core](/dotnet/azure/dotnet-quickstart-xplat)
