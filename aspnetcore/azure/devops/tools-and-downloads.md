---
title: DevOps con ASP.NET Core y Azure | Herramientas y descargas
author: CamSoper
description: Una guía que proporciona guías de un extremo a otro sobre cómo crear una canalización de DevOps para una aplicación ASP.NET Core hospedada en Azure.
ms.author: casoper
ms.date: 08/07/2018
uid: azure/devops/tools-and-downloads
ms.openlocfilehash: 3cf99f4d497bf2edd8759ab9afdee66ad49fac3d
ms.sourcegitcommit: 29dfe436f54a27fbb4f6494bc639d16c75001fab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/09/2018
ms.locfileid: "42909338"
---
# <a name="tools-and-downloads"></a><span data-ttu-id="aa8fe-103">Herramientas y descargas</span><span class="sxs-lookup"><span data-stu-id="aa8fe-103">Tools and downloads</span></span>

<span data-ttu-id="aa8fe-104">Azure tiene varias interfaces para aprovisionar y administrar los recursos, como el [portal Azure](https://portal.azure.com), [CLI de Azure](https://docs.microsoft.com/cli/azure/), [Azure PowerShell](https://docs.microsoft.com/en-us/powershell/azure/overview), [en la nube de Azure Shell](https://shell.azure.com/bash)y Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="aa8fe-104">Azure has several interfaces for provisioning and managing resources, such as the [Azure portal](https://portal.azure.com), [Azure CLI](https://docs.microsoft.com/cli/azure/), [Azure PowerShell](https://docs.microsoft.com/en-us/powershell/azure/overview), [Azure Cloud Shell](https://shell.azure.com/bash), and Visual Studio.</span></span> <span data-ttu-id="aa8fe-105">Esta guía adopta un enfoque minimalista y usa Azure Cloud Shell siempre que sea posible para reducir los pasos necesarios.</span><span class="sxs-lookup"><span data-stu-id="aa8fe-105">This guide takes a minimalist approach and uses the Azure Cloud Shell whenever possible to reduce the steps required.</span></span> <span data-ttu-id="aa8fe-106">Sin embargo, se debe usar el portal de Azure para algunas partes.</span><span class="sxs-lookup"><span data-stu-id="aa8fe-106">However, the Azure portal must be used for some portions.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="aa8fe-107">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="aa8fe-107">Prerequisites</span></span>

<span data-ttu-id="aa8fe-108">Se requieren las siguientes suscripciones:</span><span class="sxs-lookup"><span data-stu-id="aa8fe-108">The following subscriptions are required:</span></span>

* <span data-ttu-id="aa8fe-109">Azure &mdash; si no tienes una cuenta, [obtener una evaluación gratuita](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="aa8fe-109">Azure &mdash; If you don't have an account, [get a free trial](https://azure.microsoft.com/free/).</span></span>
* <span data-ttu-id="aa8fe-110">Visual Studio Team Services (VSTS) &mdash; esta cuenta se crea en el capítulo 4.</span><span class="sxs-lookup"><span data-stu-id="aa8fe-110">Visual Studio Team Services (VSTS) &mdash; This account is created in Chapter 4.</span></span>
* <span data-ttu-id="aa8fe-111">GitHub &mdash; si no tienes una cuenta, [Regístrese gratuitamente](https://github.com/join).</span><span class="sxs-lookup"><span data-stu-id="aa8fe-111">GitHub &mdash; If you don't have an account, [sign up for free](https://github.com/join).</span></span>

<span data-ttu-id="aa8fe-112">Se requieren las siguientes herramientas:</span><span class="sxs-lookup"><span data-stu-id="aa8fe-112">The following tools are required:</span></span>

* <span data-ttu-id="aa8fe-113">[GIT](https://git-scm.com/downloads) &mdash; se recomienda un entendimiento básico de Git en esta guía.</span><span class="sxs-lookup"><span data-stu-id="aa8fe-113">[Git](https://git-scm.com/downloads) &mdash; A fundamental understanding of Git is recommended for this guide.</span></span> <span data-ttu-id="aa8fe-114">Revise el [documentación de Git](https://git-scm.com/doc), concretamente [git remoto](https://git-scm.com/docs/git-remote) y [git push](https://git-scm.com/docs/git-push).</span><span class="sxs-lookup"><span data-stu-id="aa8fe-114">Review the [Git documentation](https://git-scm.com/doc), specifically [git remote](https://git-scm.com/docs/git-remote) and [git push](https://git-scm.com/docs/git-push).</span></span>
* <span data-ttu-id="aa8fe-115">[SDK de .NET core](https://www.microsoft.com/net/download/) &mdash; versión 2.1.300 o posterior, es necesario para compilar y ejecutar la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="aa8fe-115">[.NET Core SDK](https://www.microsoft.com/net/download/) &mdash; Version 2.1.300 or later is required to build and run the sample app.</span></span> <span data-ttu-id="aa8fe-116">Si está instalado Visual Studio con el **desarrollo multiplataforma de .NET Core** ya está instalada la carga de trabajo, el SDK de .NET Core.</span><span class="sxs-lookup"><span data-stu-id="aa8fe-116">If Visual Studio is installed with the **.NET Core cross-platform development** workload, the .NET Core SDK is already installed.</span></span>

    <span data-ttu-id="aa8fe-117">Compruebe la instalación del SDK de .NET Core.</span><span class="sxs-lookup"><span data-stu-id="aa8fe-117">Verify your .NET Core SDK installation.</span></span> <span data-ttu-id="aa8fe-118">Abra un shell de comandos y ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="aa8fe-118">Open a command shell, and run the following command:</span></span>

    ```console
    dotnet --version
    ```

## <a name="recommended-tools-windows-only"></a><span data-ttu-id="aa8fe-119">Herramientas recomendadas (solo Windows)</span><span class="sxs-lookup"><span data-stu-id="aa8fe-119">Recommended tools (Windows only)</span></span>

* <span data-ttu-id="aa8fe-120">[Visual Studio](https://www.visualstudio.com/)del sólidas herramientas de Azure proporcionan una interfaz gráfica de usuario para la mayoría de la funcionalidad descrita en esta guía.</span><span class="sxs-lookup"><span data-stu-id="aa8fe-120">[Visual Studio](https://www.visualstudio.com/)'s robust Azure tools provide a GUI for most of the functionality described in this guide.</span></span> <span data-ttu-id="aa8fe-121">Funcionará cualquier edición de Visual Studio, incluida la edición gratuita de Visual Studio Community.</span><span class="sxs-lookup"><span data-stu-id="aa8fe-121">Any edition of Visual Studio will work, including the free Visual Studio Community Edition.</span></span> <span data-ttu-id="aa8fe-122">Los tutoriales se escriben en demuestran el desarrollo, implementación y DevOps con y sin Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="aa8fe-122">The tutorials are written to demonstrate development, deployment, and DevOps both with and without Visual Studio.</span></span>

  <span data-ttu-id="aa8fe-123">Confirme que Visual Studio tiene las siguientes [cargas de trabajo](https://docs.microsoft.com/visualstudio/install/modify-visual-studio) instalado:</span><span class="sxs-lookup"><span data-stu-id="aa8fe-123">Confirm that Visual Studio has the following [workloads](https://docs.microsoft.com/visualstudio/install/modify-visual-studio) installed:</span></span>

  * <span data-ttu-id="aa8fe-124">Desarrollo web y ASP.NET</span><span class="sxs-lookup"><span data-stu-id="aa8fe-124">ASP.NET and web development</span></span>
  * <span data-ttu-id="aa8fe-125">Desarrollo de Azure</span><span class="sxs-lookup"><span data-stu-id="aa8fe-125">Azure development</span></span>
  * <span data-ttu-id="aa8fe-126">Desarrollo multiplataforma de .NET Core</span><span class="sxs-lookup"><span data-stu-id="aa8fe-126">.NET Core cross-platform development</span></span>
