---
title: "Publicación de una aplicación de ASP.NET Core en Azure con las herramientas de línea de comandos | Microsoft Docs"
description: "Obtenga información sobre cómo compilar e implementar una aplicación de Microsoft Azure con ASP.NET Core y el cliente de línea de comandos de Git."
services: multiple
keywords: "ASP.NET Core, Azure, App Service, Git, línea de comandos"
author: camsoper
ms.author: casoper
manager: wpickett
ms.date: 11/03/2017
ms.topic: get-started-article
ms.prod: asp.net-core
ms.technology: aspnet
ms.custom: mvc
ms.devlang: dotnet
uid: tutorials/publish-to-azure-webapp-using-cli
ms.openlocfilehash: 0bcff4f79356b960f663dcebb1d79a108417dbd2
ms.sourcegitcommit: f017f940a164dbaf84307410c78eb14e0f3ac811
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2017
---
# <a name="deploy-an-aspnet-core-application-to-azure-app-service-from-the-command-line"></a>Implementación de una aplicación de ASP.NET Core en Azure App Service desde la línea de comandos

Por [Cam Soper](https://twitter.com/camsoper)

En este tutorial se explica cómo compilar e implementar una aplicación de ASP.NET Core en Microsoft Azure App Service con las herramientas de línea de comandos.  Cuando termine, tendrá una aplicación web integrada en ASP.NET MVC Core hospedada como una aplicación web de Azure App Service.  Este tutorial se escribe con herramientas de línea de comandos de Windows, pero también puede aplicarse a entornos de macOS y Linux.  

En este tutorial aprenderá a:

> [!div class="checklist"]
> * Crear un sitio web en Azure App Service con la CLI de Azure
> * Implementar una aplicación de ASP.NET Core en Azure App Service con la herramienta de línea de comandos de Git

## <a name="prerequisites"></a>Requisitos previos

Para completar este tutorial, necesita:

* Una [suscripción de Microsoft Azure](https://azure.microsoft.com/free/)
* [Núcleo de .NET](https://www.microsoft.com/net/download/core)
* Un cliente de línea de comandos de [Git](https://www.git-scm.com/)

## <a name="create-a-web-application"></a>Creación de una aplicación web

Cree un directorio para la aplicación web, cree una aplicación de ASP.NET Core MVC y luego ejecute el sitio web de forma local.

# <a name="windowstabwindows"></a>[Windows](#tab/windows)
```cmd
REM Create a new ASP.NET Core MVC application
dotnet new razor -o MyApplication

REM Change to the new directory that was just created
cd MyApplication

REM Run the application
dotnet run
```

# <a name="othertabother"></a>[Otros problemas](#tab/other)
```bash
# Create a new ASP.NET Core MVC application
dotnet new razor -o MyApplication

# Change to the new directory that was just created
cd MyApplication

# Run the application
dotnet run
```
---

![Salida de la línea de comandos](publish-to-azure-webapp-using-cli/_static/new_prj.png)

Navegue a http://localhost:5000 para probar la aplicación.

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

Se necesita una dirección URL de implementación para implementar la aplicación con Git.  Recupere una dirección URL como esta.

```azurecli-interactive
az webapp deployment source config-local-git -n $webappname -g DotNetAzureTutorial --query [url] -o tsv
```
Anote la dirección URL mostrada que termina en `.git`. Se utiliza en el paso siguiente.

## <a name="deploy-the-application-using-git"></a>Implementación de la aplicación con Git

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

Git solicitará las credenciales de implementación establecidas anteriormente.  Tras la autenticación, la aplicación se inserta en la ubicación remota, se compila y se implementa.

![Resultado de la implementación de Git](publish-to-azure-webapp-using-cli/_static/post_deploy.png)

## <a name="test-the-application"></a>Probar la aplicación

Navegue a `https://<web app name>.azurewebsites.net` para probar la aplicación.  Para mostrar la dirección en Cloud Shell o en la CLI de Azure, se usa lo siguiente:

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
> * Implementar una aplicación de ASP.NET Core en Azure App Service con la herramienta de línea de comandos de Git

A continuación, aprenda a usar la línea de comandos para implementar una aplicación web existente que usa Cosmos DB.

> [!div class="nextstepaction"]
> [Implementación en Azure desde la línea de comandos con .NET Core](/dotnet/azure/dotnet-quickstart-xplat)
