---
title: Herramientas y descargas - DevOps con ASP.NET Core y Azure
author: CamSoper
description: Herramientas y descargas necesarias para DevOps con ASP.NET Core y Azure.
ms.author: casoper
ms.custom: mvc, seodec18
ms.date: 10/24/2018
uid: azure/devops/tools-and-downloads
ms.openlocfilehash: ae4827f735274405021e5ee539d1029b7ddb9553
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/27/2019
ms.locfileid: "64891992"
---
# <a name="tools-and-downloads"></a><span data-ttu-id="4cc0c-103">Herramientas y descargas</span><span class="sxs-lookup"><span data-stu-id="4cc0c-103">Tools and downloads</span></span>

<span data-ttu-id="4cc0c-104">Azure tiene varias interfaces para aprovisionar y administrar los recursos, como el [portal Azure](https://portal.azure.com), [CLI de Azure](/cli/azure/), [Azure PowerShell](/powershell/azure/overview), [en la nube de Azure Shell](https://shell.azure.com/bash)y Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4cc0c-104">Azure has several interfaces for provisioning and managing resources, such as the [Azure portal](https://portal.azure.com), [Azure CLI](/cli/azure/), [Azure PowerShell](/powershell/azure/overview), [Azure Cloud Shell](https://shell.azure.com/bash), and Visual Studio.</span></span> <span data-ttu-id="4cc0c-105">Esta guía adopta un enfoque minimalista y usa Azure Cloud Shell siempre que sea posible para reducir los pasos necesarios.</span><span class="sxs-lookup"><span data-stu-id="4cc0c-105">This guide takes a minimalist approach and uses the Azure Cloud Shell whenever possible to reduce the steps required.</span></span> <span data-ttu-id="4cc0c-106">Sin embargo, se debe usar el portal de Azure para algunas partes.</span><span class="sxs-lookup"><span data-stu-id="4cc0c-106">However, the Azure portal must be used for some portions.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4cc0c-107">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="4cc0c-107">Prerequisites</span></span>

<span data-ttu-id="4cc0c-108">Se requieren las siguientes suscripciones:</span><span class="sxs-lookup"><span data-stu-id="4cc0c-108">The following subscriptions are required:</span></span>

* <span data-ttu-id="4cc0c-109">Azure &mdash; si no tienes una cuenta, [obtener una evaluación gratuita](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="4cc0c-109">Azure &mdash; If you don't have an account, [get a free trial](https://azure.microsoft.com/free/).</span></span>
* <span data-ttu-id="4cc0c-110">Servicios de Azure DevOps &mdash; su suscripción de Azure DevOps y la organización se crea en el capítulo 4.</span><span class="sxs-lookup"><span data-stu-id="4cc0c-110">Azure DevOps Services &mdash; your Azure DevOps subscription and organization is created in Chapter 4.</span></span>
* <span data-ttu-id="4cc0c-111">GitHub &mdash; si no tienes una cuenta, [Regístrese gratuitamente](https://github.com/join).</span><span class="sxs-lookup"><span data-stu-id="4cc0c-111">GitHub &mdash; If you don't have an account, [sign up for free](https://github.com/join).</span></span>

<span data-ttu-id="4cc0c-112">Se requieren las siguientes herramientas:</span><span class="sxs-lookup"><span data-stu-id="4cc0c-112">The following tools are required:</span></span>

* <span data-ttu-id="4cc0c-113">[GIT](https://git-scm.com/downloads) &mdash; se recomienda un entendimiento básico de Git en esta guía.</span><span class="sxs-lookup"><span data-stu-id="4cc0c-113">[Git](https://git-scm.com/downloads) &mdash; A fundamental understanding of Git is recommended for this guide.</span></span> <span data-ttu-id="4cc0c-114">Revise el [documentación de Git](https://git-scm.com/doc), concretamente [git remoto](https://git-scm.com/docs/git-remote) y [git push](https://git-scm.com/docs/git-push).</span><span class="sxs-lookup"><span data-stu-id="4cc0c-114">Review the [Git documentation](https://git-scm.com/doc), specifically [git remote](https://git-scm.com/docs/git-remote) and [git push](https://git-scm.com/docs/git-push).</span></span>
* <span data-ttu-id="4cc0c-115">[SDK de .NET core](https://www.microsoft.com/net/download/) &mdash; versión 2.1.300 o posterior, es necesario para compilar y ejecutar la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="4cc0c-115">[.NET Core SDK](https://www.microsoft.com/net/download/) &mdash; Version 2.1.300 or later is required to build and run the sample app.</span></span> <span data-ttu-id="4cc0c-116">Si está instalado Visual Studio con el **desarrollo multiplataforma de .NET Core** ya está instalada la carga de trabajo, el SDK de .NET Core.</span><span class="sxs-lookup"><span data-stu-id="4cc0c-116">If Visual Studio is installed with the **.NET Core cross-platform development** workload, the .NET Core SDK is already installed.</span></span>

    <span data-ttu-id="4cc0c-117">Compruebe la instalación del SDK de .NET Core.</span><span class="sxs-lookup"><span data-stu-id="4cc0c-117">Verify your .NET Core SDK installation.</span></span> <span data-ttu-id="4cc0c-118">Abra un shell de comandos y ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="4cc0c-118">Open a command shell, and run the following command:</span></span>

    ```console
    dotnet --version
    ```

## <a name="recommended-tools-windows-only"></a><span data-ttu-id="4cc0c-119">Herramientas recomendadas (solo Windows)</span><span class="sxs-lookup"><span data-stu-id="4cc0c-119">Recommended tools (Windows only)</span></span>

* <span data-ttu-id="4cc0c-120">[Visual Studio](https://visualstudio.microsoft.com)del sólidas herramientas de Azure proporcionan una interfaz gráfica de usuario para la mayoría de la funcionalidad descrita en esta guía.</span><span class="sxs-lookup"><span data-stu-id="4cc0c-120">[Visual Studio](https://visualstudio.microsoft.com)'s robust Azure tools provide a GUI for most of the functionality described in this guide.</span></span> <span data-ttu-id="4cc0c-121">Funcionará cualquier edición de Visual Studio, incluida la edición gratuita de Visual Studio Community.</span><span class="sxs-lookup"><span data-stu-id="4cc0c-121">Any edition of Visual Studio will work, including the free Visual Studio Community Edition.</span></span> <span data-ttu-id="4cc0c-122">Los tutoriales se escriben en demuestran el desarrollo, implementación y DevOps con y sin Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4cc0c-122">The tutorials are written to demonstrate development, deployment, and DevOps both with and without Visual Studio.</span></span>

  <span data-ttu-id="4cc0c-123">Confirme que Visual Studio tiene las siguientes [cargas de trabajo](/visualstudio/install/modify-visual-studio) instalado:</span><span class="sxs-lookup"><span data-stu-id="4cc0c-123">Confirm that Visual Studio has the following [workloads](/visualstudio/install/modify-visual-studio) installed:</span></span>

  * <span data-ttu-id="4cc0c-124">Desarrollo web y ASP.NET</span><span class="sxs-lookup"><span data-stu-id="4cc0c-124">ASP.NET and web development</span></span>
  * <span data-ttu-id="4cc0c-125">Desarrollo de Azure</span><span class="sxs-lookup"><span data-stu-id="4cc0c-125">Azure development</span></span>
  * <span data-ttu-id="4cc0c-126">Desarrollo multiplataforma de .NET Core</span><span class="sxs-lookup"><span data-stu-id="4cc0c-126">.NET Core cross-platform development</span></span>
