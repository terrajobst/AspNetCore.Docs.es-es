---
uid: whitepapers/aspnet-mvc2-upgrade-notes
title: Actualizar una aplicación de ASP.NET MVC 1.0 a ASP.NET MVC 2 | Microsoft Docs
author: rick-anderson
description: Este documento explica cómo actualizar manualmente y con un Asistente para una aplicación ASP.NET MVC 1.0 a ASP.NET MVC 2. Este documento también está disponible para d...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/08/2010
ms.topic: article
ms.assetid: f1a01759-d251-4b09-8835-e112e336c6dd
ms.technology: ''
msc.legacyurl: /whitepapers/aspnet-mvc2-upgrade-notes
msc.type: content
ms.openlocfilehash: 354dbab3057068eb13b16c9aa31f66c72371ce7e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37381921"
---
<a name="upgrading-an-aspnet-mvc-10-application-to-aspnet-mvc-2"></a><span data-ttu-id="260e3-104">Actualizar una aplicación de ASP.NET MVC 1.0 a ASP.NET MVC 2</span><span class="sxs-lookup"><span data-stu-id="260e3-104">Upgrading an ASP.NET MVC 1.0 Application to ASP.NET MVC 2</span></span>
====================
> <span data-ttu-id="260e3-105">Este documento explica cómo actualizar manualmente y con un Asistente para una aplicación ASP.NET MVC 1.0 a ASP.NET MVC 2.</span><span class="sxs-lookup"><span data-stu-id="260e3-105">This document describes both how to upgrade manually and with a wizard an ASP.NET MVC 1.0 Application to ASP.NET MVC 2.</span></span> <span data-ttu-id="260e3-106">Este documento también está disponible para [descargar](https://download.microsoft.com/download/F/1/6/F16F9AF9-8EF4-4845-BC97-639791D5699C/MVC2-Upgrade-Notes.pdf)</span><span class="sxs-lookup"><span data-stu-id="260e3-106">This document is also available for [Download](https://download.microsoft.com/download/F/1/6/F16F9AF9-8EF4-4845-BC97-639791D5699C/MVC2-Upgrade-Notes.pdf)</span></span>


## <a name="introduction"></a><span data-ttu-id="260e3-107">Introducción</span><span class="sxs-lookup"><span data-stu-id="260e3-107">Introduction</span></span>

<span data-ttu-id="260e3-108">ASP.NET MVC 2 se puede instalar en paralelo con ASP.NET MVC 1.0 en el mismo servidor.</span><span class="sxs-lookup"><span data-stu-id="260e3-108">ASP.NET MVC 2 can be installed side by side with ASP.NET MVC 1.0 on the same server.</span></span> <span data-ttu-id="260e3-109">Esto proporciona flexibilidad a los desarrolladores de aplicaciones en elegir cuándo se debe actualizar una aplicación de ASP.NET MVC 1.0 a ASP.NET MVC 2.</span><span class="sxs-lookup"><span data-stu-id="260e3-109">This gives application developers flexibility in choosing when to upgrade an ASP.NET MVC 1.0 application to ASP.NET MVC 2.</span></span>

<span data-ttu-id="260e3-110">Visual Studio 2010 incluye a un asistente que las actualizaciones de los proyectos de ASP.NET MVC 1.0 existentes creados con Visual Studio 2008 para ASP.NET MVC 2.</span><span class="sxs-lookup"><span data-stu-id="260e3-110">Visual Studio 2010 includes a wizard that upgrades existing ASP.NET MVC 1.0 projects built with Visual Studio 2008 to ASP.NET MVC 2.</span></span> <span data-ttu-id="260e3-111">Se inicia el Asistente para actualización, abra un proyecto de ASP.NET MVC 1.0 en Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="260e3-111">The upgrade wizard is initiated by opening an ASP.NET MVC 1.0 project in Visual Studio 2010.</span></span>

## <a name="upgrade-wizard-for-aspnet-mvc-10-on-visual-studio-2008-sp1"></a><span data-ttu-id="260e3-112">Actualizar el Asistente para ASP.NET MVC 1.0 en Visual Studio 2008 SP1</span><span class="sxs-lookup"><span data-stu-id="260e3-112">Upgrade Wizard for ASP.NET MVC 1.0 on Visual Studio 2008 SP1</span></span>

<span data-ttu-id="260e3-113">Para actualizar una aplicación de ASP.NET MVC 1.0 a ASP.NET MVC 2 en Visual Studio 2008 SP1, use la aplicación MvcAppConverter (no compatible).</span><span class="sxs-lookup"><span data-stu-id="260e3-113">To upgrade an ASP.NET MVC 1.0 application to ASP.NET MVC 2 in Visual Studio 2008 SP1, use the (unsupported) MvcAppConverter application.</span></span> <span data-ttu-id="260e3-114">Puede descargar esta aplicación desde la dirección URL siguiente:</span><span class="sxs-lookup"><span data-stu-id="260e3-114">You can download this application from the following URL:</span></span>

[https://go.microsoft.com/fwlink/?LinkID=185351](https://go.microsoft.com/fwlink/?LinkID=185351)

## <a name="manually-upgrading-an-aspnet-mvc-10-project"></a><span data-ttu-id="260e3-115">Actualizar manualmente un proyecto de ASP.NET MVC 1.0</span><span class="sxs-lookup"><span data-stu-id="260e3-115">Manually Upgrading an ASP.NET MVC 1.0 Project</span></span>

<span data-ttu-id="260e3-116">Para actualizar manualmente una aplicación existente de ASP.NET MVC 1.0 a la versión 2, siga estos pasos:</span><span class="sxs-lookup"><span data-stu-id="260e3-116">To manually upgrade an existing ASP.NET MVC 1.0 application to version 2, follow these steps:</span></span>

1. <span data-ttu-id="260e3-117">Realice una copia de seguridad del proyecto existente.</span><span class="sxs-lookup"><span data-stu-id="260e3-117">Make a backup of the existing project.</span></span>
2. <span data-ttu-id="260e3-118">En un editor de texto, abra el archivo de proyecto (el archivo con la extensión de archivo .csproj o .vbproj) y busque el elemento ProjectTypeGuid.</span><span class="sxs-lookup"><span data-stu-id="260e3-118">In a text editor, open the project file (the file with the .csproj or .vbproj file extension) and find the ProjectTypeGuid element.</span></span> <span data-ttu-id="260e3-119">Como el valor de ese elemento, reemplace el GUID {603c0e0b-db56-11dc-be95-000d561079b0} con {F85E285D-A4E0-4152-9332-AB1D724D3325}.</span><span class="sxs-lookup"><span data-stu-id="260e3-119">As the value of that element, replace the GUID {603c0e0b-db56-11dc-be95-000d561079b0} with {F85E285D-A4E0-4152-9332-AB1D724D3325}.</span></span> <span data-ttu-id="260e3-120">Cuando haya terminado, el valor de ese elemento debe ser como sigue:</span><span class="sxs-lookup"><span data-stu-id="260e3-120">When you are done, the value of that element should be as follows:</span></span> 

    `{F85E285D-A4E0-4152-9332-AB1D724D3325};{349c5851-65df-11da-9384-00065b846f21};{fae04ec0-301f-11d3-bf4b-00c04f79efbc}`
3. <span data-ttu-id="260e3-121">En la carpeta raíz de aplicación Web, edite el archivo Web.config.</span><span class="sxs-lookup"><span data-stu-id="260e3-121">In the Web application root folder, edit the Web.config file.</span></span> <span data-ttu-id="260e3-122">Busque System.Web.Mvc, versión = 1.0.0.0 y reemplace todas las instancias con System.Web.Mvc, versión = 2.0.0.0.</span><span class="sxs-lookup"><span data-stu-id="260e3-122">Search for System.Web.Mvc, Version=1.0.0.0 and replace all instances with System.Web.Mvc, Version=2.0.0.0.</span></span>
4. <span data-ttu-id="260e3-123">Repita el paso anterior para el archivo Web.config ubicado en la carpeta Views.</span><span class="sxs-lookup"><span data-stu-id="260e3-123">Repeat the previous step for the Web.config file located in the Views folder.</span></span>
5. <span data-ttu-id="260e3-124">Abra el proyecto con Visual Studio y en **el Explorador de soluciones**, expanda el **referencias** nodo.</span><span class="sxs-lookup"><span data-stu-id="260e3-124">Open the project using Visual Studio, and in **Solution Explorer**, expand the **References** node.</span></span> <span data-ttu-id="260e3-125">Elimine la referencia a System.Web.Mvc (que apunta al ensamblado de la versión 1.0).</span><span class="sxs-lookup"><span data-stu-id="260e3-125">Delete the reference to System.Web.Mvc (which points to the version 1.0 assembly).</span></span> <span data-ttu-id="260e3-126">Agregue una referencia a System.Web.Mvc (v2.0.0.0).</span><span class="sxs-lookup"><span data-stu-id="260e3-126">Add a reference to System.Web.Mvc (v2.0.0.0).</span></span>
6. <span data-ttu-id="260e3-127">Agregue el siguiente elemento bindingRedirect al archivo Web.config en la raíz de la aplicación en la sección de configuración:</span><span class="sxs-lookup"><span data-stu-id="260e3-127">Add the following bindingRedirect element to the Web.config file in the application root under the configuraton section:</span></span>   

    [!code-xml[Main](aspnet-mvc2-upgrade-notes/samples/sample1.xml)]
7. <span data-ttu-id="260e3-128">Cree una nueva aplicación vacía de ASP.NET MVC 2.</span><span class="sxs-lookup"><span data-stu-id="260e3-128">Create a new empty ASP.NET MVC 2 application.</span></span> <span data-ttu-id="260e3-129">Copie los archivos de la carpeta de Scripts de la nueva aplicación en la carpeta de Scripts de la aplicación existente.</span><span class="sxs-lookup"><span data-stu-id="260e3-129">Copy the files from the Scripts folder of the new application into the Scripts folder of the existing application.</span></span>
8. <span data-ttu-id="260e3-130">Actualizar la existente es decir™ archivo CSS de s con las definiciones de estilos CSS en el archivo Site.css.</span><span class="sxs-lookup"><span data-stu-id="260e3-130">Update the existing applicationâ€™s CSS file with the CSS style definitions in the Site.css file.</span></span>
9. <span data-ttu-id="260e3-131">Compile la aplicación y ejecutarla.</span><span class="sxs-lookup"><span data-stu-id="260e3-131">Compile the application and run it.</span></span> <span data-ttu-id="260e3-132">Si se produce algún error, consulte la sección cambios importantes de la [Novedades de ASP.NET MVC 2](https://go.microsoft.com/fwlink/?LinkID=185038) página.</span><span class="sxs-lookup"><span data-stu-id="260e3-132">If any errors occur, refer to the Breaking Changes section of the [What's New in ASP.NET MVC 2](https://go.microsoft.com/fwlink/?LinkID=185038) page.</span></span>
