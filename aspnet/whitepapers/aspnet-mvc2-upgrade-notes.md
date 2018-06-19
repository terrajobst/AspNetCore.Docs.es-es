---
uid: whitepapers/aspnet-mvc2-upgrade-notes
title: Actualizar una aplicación de MVC de ASP.NET 1.0 a ASP.NET MVC 2 | Documentos de Microsoft
author: rick-anderson
description: Este documento explica cómo actualizar manualmente y con el Asistente para una aplicación ASP.NET MVC 1.0 para ASP.NET MVC 2. Este documento también está disponible para d...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/08/2010
ms.topic: article
ms.assetid: f1a01759-d251-4b09-8835-e112e336c6dd
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /whitepapers/aspnet-mvc2-upgrade-notes
msc.type: content
ms.openlocfilehash: d1227f0738b2d2a396fed942f32ae5e75579596e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
ms.locfileid: "26530064"
---
<a name="upgrading-an-aspnet-mvc-10-application-to-aspnet-mvc-2"></a><span data-ttu-id="fe48f-104">Actualizar una aplicación de MVC de ASP.NET 1.0 a ASP.NET MVC 2</span><span class="sxs-lookup"><span data-stu-id="fe48f-104">Upgrading an ASP.NET MVC 1.0 Application to ASP.NET MVC 2</span></span>
====================
> <span data-ttu-id="fe48f-105">Este documento explica cómo actualizar manualmente y con el Asistente para una aplicación ASP.NET MVC 1.0 para ASP.NET MVC 2.</span><span class="sxs-lookup"><span data-stu-id="fe48f-105">This document describes both how to upgrade manually and with a wizard an ASP.NET MVC 1.0 Application to ASP.NET MVC 2.</span></span> <span data-ttu-id="fe48f-106">Este documento también está disponible para [descargar](https://download.microsoft.com/download/F/1/6/F16F9AF9-8EF4-4845-BC97-639791D5699C/MVC2-Upgrade-Notes.pdf)</span><span class="sxs-lookup"><span data-stu-id="fe48f-106">This document is also available for [Download](https://download.microsoft.com/download/F/1/6/F16F9AF9-8EF4-4845-BC97-639791D5699C/MVC2-Upgrade-Notes.pdf)</span></span>


## <a name="introduction"></a><span data-ttu-id="fe48f-107">Introducción</span><span class="sxs-lookup"><span data-stu-id="fe48f-107">Introduction</span></span>

<span data-ttu-id="fe48f-108">ASP.NET MVC 2 puede instalarse paralelo con ASP.NET MVC 1.0 en el mismo servidor.</span><span class="sxs-lookup"><span data-stu-id="fe48f-108">ASP.NET MVC 2 can be installed side by side with ASP.NET MVC 1.0 on the same server.</span></span> <span data-ttu-id="fe48f-109">Esto proporciona flexibilidad a los desarrolladores de aplicaciones en elegir cuándo se debe actualizar una aplicación de ASP.NET MVC 1.0 para ASP.NET MVC 2.</span><span class="sxs-lookup"><span data-stu-id="fe48f-109">This gives application developers flexibility in choosing when to upgrade an ASP.NET MVC 1.0 application to ASP.NET MVC 2.</span></span>

<span data-ttu-id="fe48f-110">Visual Studio 2010 incluye a un asistente que las actualizaciones de los proyectos de ASP.NET MVC 1.0 existentes compilados con Visual Studio 2008 para ASP.NET MVC 2.</span><span class="sxs-lookup"><span data-stu-id="fe48f-110">Visual Studio 2010 includes a wizard that upgrades existing ASP.NET MVC 1.0 projects built with Visual Studio 2008 to ASP.NET MVC 2.</span></span> <span data-ttu-id="fe48f-111">Se inicia el Asistente para actualización, abra un proyecto de ASP.NET MVC 1.0 en Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="fe48f-111">The upgrade wizard is initiated by opening an ASP.NET MVC 1.0 project in Visual Studio 2010.</span></span>

## <a name="upgrade-wizard-for-aspnet-mvc-10-on-visual-studio-2008-sp1"></a><span data-ttu-id="fe48f-112">Actualizar el Asistente para ASP.NET MVC 1.0 en Visual Studio 2008 SP1</span><span class="sxs-lookup"><span data-stu-id="fe48f-112">Upgrade Wizard for ASP.NET MVC 1.0 on Visual Studio 2008 SP1</span></span>

<span data-ttu-id="fe48f-113">Para actualizar una aplicación de ASP.NET MVC 1.0 para ASP.NET MVC 2 de Visual Studio 2008 SP1, use la aplicación de MvcAppConverter (no compatible).</span><span class="sxs-lookup"><span data-stu-id="fe48f-113">To upgrade an ASP.NET MVC 1.0 application to ASP.NET MVC 2 in Visual Studio 2008 SP1, use the (unsupported) MvcAppConverter application.</span></span> <span data-ttu-id="fe48f-114">Puede descargar esta aplicación desde la dirección URL siguiente:</span><span class="sxs-lookup"><span data-stu-id="fe48f-114">You can download this application from the following URL:</span></span>

[<span data-ttu-id="fe48f-115">https://go.Microsoft.com/fwlink/?LinkId=185351</span><span class="sxs-lookup"><span data-stu-id="fe48f-115">https://go.microsoft.com/fwlink/?LinkID=185351</span></span>](https://go.microsoft.com/fwlink/?LinkID=185351)

## <a name="manually-upgrading-an-aspnet-mvc-10-project"></a><span data-ttu-id="fe48f-116">Actualizar manualmente un proyecto de MVC de ASP.NET 1.0</span><span class="sxs-lookup"><span data-stu-id="fe48f-116">Manually Upgrading an ASP.NET MVC 1.0 Project</span></span>

<span data-ttu-id="fe48f-117">Para actualizar manualmente una aplicación existente de ASP.NET MVC 1.0 a la versión 2, siga estos pasos:</span><span class="sxs-lookup"><span data-stu-id="fe48f-117">To manually upgrade an existing ASP.NET MVC 1.0 application to version 2, follow these steps:</span></span>

1. <span data-ttu-id="fe48f-118">Realizar una copia de seguridad del proyecto existente.</span><span class="sxs-lookup"><span data-stu-id="fe48f-118">Make a backup of the existing project.</span></span>
2. <span data-ttu-id="fe48f-119">En un editor de texto, abra el archivo de proyecto (el archivo con la extensión de archivo .csproj o .vbproj) y busque el elemento ProjectTypeGuid.</span><span class="sxs-lookup"><span data-stu-id="fe48f-119">In a text editor, open the project file (the file with the .csproj or .vbproj file extension) and find the ProjectTypeGuid element.</span></span> <span data-ttu-id="fe48f-120">Según el valor de ese elemento, reemplace el GUID {603c0e0b-db56-11dc-be95-000d561079b0} con {F85E285D-A4E0-4152-9332-AB1D724D3325}.</span><span class="sxs-lookup"><span data-stu-id="fe48f-120">As the value of that element, replace the GUID {603c0e0b-db56-11dc-be95-000d561079b0} with {F85E285D-A4E0-4152-9332-AB1D724D3325}.</span></span> <span data-ttu-id="fe48f-121">Cuando haya terminado, el valor de ese elemento debe ser como sigue:</span><span class="sxs-lookup"><span data-stu-id="fe48f-121">When you are done, the value of that element should be as follows:</span></span> 

    `{F85E285D-A4E0-4152-9332-AB1D724D3325};{349c5851-65df-11da-9384-00065b846f21};{fae04ec0-301f-11d3-bf4b-00c04f79efbc}`
3. <span data-ttu-id="fe48f-122">En la carpeta raíz de aplicación Web, edite el archivo Web.config.</span><span class="sxs-lookup"><span data-stu-id="fe48f-122">In the Web application root folder, edit the Web.config file.</span></span> <span data-ttu-id="fe48f-123">Busque System.Web.Mvc, versión = 1.0.0.0 y reemplace todas las instancias con System.Web.Mvc, versión = 2.0.0.0.</span><span class="sxs-lookup"><span data-stu-id="fe48f-123">Search for System.Web.Mvc, Version=1.0.0.0 and replace all instances with System.Web.Mvc, Version=2.0.0.0.</span></span>
4. <span data-ttu-id="fe48f-124">Repita el paso anterior para el archivo Web.config ubicado en la carpeta Views.</span><span class="sxs-lookup"><span data-stu-id="fe48f-124">Repeat the previous step for the Web.config file located in the Views folder.</span></span>
5. <span data-ttu-id="fe48f-125">Abra el proyecto con Visual Studio y en **el Explorador de soluciones**, expanda la **referencias** nodo.</span><span class="sxs-lookup"><span data-stu-id="fe48f-125">Open the project using Visual Studio, and in **Solution Explorer**, expand the **References** node.</span></span> <span data-ttu-id="fe48f-126">Elimine la referencia a System.Web.Mvc (que señala el ensamblado de la versión 1.0).</span><span class="sxs-lookup"><span data-stu-id="fe48f-126">Delete the reference to System.Web.Mvc (which points to the version 1.0 assembly).</span></span> <span data-ttu-id="fe48f-127">Agregue una referencia a System.Web.Mvc (v2.0.0.0).</span><span class="sxs-lookup"><span data-stu-id="fe48f-127">Add a reference to System.Web.Mvc (v2.0.0.0).</span></span>
6. <span data-ttu-id="fe48f-128">Agregue el siguiente elemento bindingRedirect en el archivo Web.config en la raíz de la aplicación en la sección de configuración:</span><span class="sxs-lookup"><span data-stu-id="fe48f-128">Add the following bindingRedirect element to the Web.config file in the application root under the configuraton section:</span></span>   

    [!code-xml[Main](aspnet-mvc2-upgrade-notes/samples/sample1.xml)]
7. <span data-ttu-id="fe48f-129">Cree una nueva aplicación de ASP.NET MVC 2 vacía.</span><span class="sxs-lookup"><span data-stu-id="fe48f-129">Create a new empty ASP.NET MVC 2 application.</span></span> <span data-ttu-id="fe48f-130">Copie los archivos de la carpeta de Scripts de la nueva aplicación en la carpeta de Scripts de la aplicación existente.</span><span class="sxs-lookup"><span data-stu-id="fe48f-130">Copy the files from the Scripts folder of the new application into the Scripts folder of the existing application.</span></span>
8. <span data-ttu-id="fe48f-131">Actualizar el existente es decir™ archivo CSS de s con las definiciones de estilos CSS en el archivo Site.css.</span><span class="sxs-lookup"><span data-stu-id="fe48f-131">Update the existing applicationâ€™s CSS file with the CSS style definitions in the Site.css file.</span></span>
9. <span data-ttu-id="fe48f-132">Compilar la aplicación y ejecutarlo.</span><span class="sxs-lookup"><span data-stu-id="fe48f-132">Compile the application and run it.</span></span> <span data-ttu-id="fe48f-133">Si se produce algún error, consulte la sección de cambios importantes de la [What's New en ASP.NET MVC 2](https://go.microsoft.com/fwlink/?LinkID=185038) página.</span><span class="sxs-lookup"><span data-stu-id="fe48f-133">If any errors occur, refer to the Breaking Changes section of the [What's New in ASP.NET MVC 2](https://go.microsoft.com/fwlink/?LinkID=185038) page.</span></span>
