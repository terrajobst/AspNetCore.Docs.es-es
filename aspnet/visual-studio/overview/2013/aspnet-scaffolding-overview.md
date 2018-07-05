---
uid: visual-studio/overview/2013/aspnet-scaffolding-overview
title: Scaffolding de ASP.NET en Visual Studio 2013 | Microsoft Docs
author: tfitzmac
description: Scaffolding de ASP.NET es una característica nueva que se incluye en Visual Studio 2013.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/09/2014
ms.topic: article
ms.assetid: a41ec9d4-8287-4f31-9e2a-460e7b7f04be
ms.technology: ''
msc.legacyurl: /visual-studio/overview/2013/aspnet-scaffolding-overview
msc.type: authoredcontent
ms.openlocfilehash: f014ef1439376349f49f10f1f4063945ab81e7c1
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37369097"
---
<a name="aspnet-scaffolding-in-visual-studio-2013"></a><span data-ttu-id="8076f-103">Scaffolding de ASP.NET en Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="8076f-103">ASP.NET Scaffolding in Visual Studio 2013</span></span>
====================
<span data-ttu-id="8076f-104">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="8076f-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="8076f-105">Scaffolding de ASP.NET es una característica nueva que se incluye en Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="8076f-105">ASP.NET Scaffolding is a new feature that is included in Visual Studio 2013.</span></span>


## <a name="overview"></a><span data-ttu-id="8076f-106">Información general</span><span class="sxs-lookup"><span data-stu-id="8076f-106">Overview</span></span>

<span data-ttu-id="8076f-107">Scaffolding de ASP.NET es un marco de generación de código para aplicaciones Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="8076f-107">ASP.NET Scaffolding is a code generation framework for ASP.NET Web applications.</span></span> <span data-ttu-id="8076f-108">Visual Studio 2013 incluye generadores de código instalado previamente para los proyectos de MVC y Web API.</span><span class="sxs-lookup"><span data-stu-id="8076f-108">Visual Studio 2013 includes pre-installed code generators for MVC and Web API projects.</span></span> <span data-ttu-id="8076f-109">Agregar scaffolding al proyecto cuando desea agregar rápidamente código que interactúe con modelos de datos.</span><span class="sxs-lookup"><span data-stu-id="8076f-109">You add scaffolding to your project when you want to quickly add code that interacts with data models.</span></span> <span data-ttu-id="8076f-110">Usar scaffolding puede reducir la cantidad de tiempo para desarrollar las operaciones de datos estándar en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="8076f-110">Using scaffolding can reduce the amount of time to develop standard data operations in your project.</span></span>

<span data-ttu-id="8076f-111">De forma predeterminada, Visual Studio 2013 no admite la generación de código para un proyecto de formularios Web Forms, pero puede usar con formularios Web Forms scaffolding agregando las dependencias MVC al proyecto o instalar una extensión.</span><span class="sxs-lookup"><span data-stu-id="8076f-111">By default, Visual Studio 2013 does not support generating code for a Web Forms project, but you can use scaffolding with Web Forms by either adding MVC dependencies to the project or installing an extension.</span></span> <span data-ttu-id="8076f-112">A continuación se muestran ambos enfoques.</span><span class="sxs-lookup"><span data-stu-id="8076f-112">Both approaches are shown below.</span></span>

<span data-ttu-id="8076f-113">Visual Studio 2013 Update 2 (actualmente RC) proporciona la capacidad para ampliar el Scaffolding de ASP.NET para cumplir los requisitos de su escenario.</span><span class="sxs-lookup"><span data-stu-id="8076f-113">Visual Studio 2013 Update 2 (currently RC) provides the ability to extend ASP.NET Scaffolding to meet the requirements of your scenario.</span></span> <span data-ttu-id="8076f-114">Con esta funcionalidad, puede crear una plantilla de scaffolding personalizado y agregarlo al cuadro de diálogo Agregar Scaffold de nuevo.</span><span class="sxs-lookup"><span data-stu-id="8076f-114">With this functionality, you can create a customized scaffolding template and add it to Add New Scaffold dialog.</span></span> <span data-ttu-id="8076f-115">Dentro de la plantilla personalizada, especifique el código que se genera cuando se agrega un elemento con scaffolding.</span><span class="sxs-lookup"><span data-stu-id="8076f-115">Within the customized template, you specify the code that is generated when adding a scaffolded item.</span></span> <span data-ttu-id="8076f-116">Para obtener más información, consulte [creación de un proveedor de scaffolding personalizado para Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).</span><span class="sxs-lookup"><span data-stu-id="8076f-116">For more information, see [Creating a Custom Scaffolder for Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8076f-117">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="8076f-117">Prerequisites</span></span>

<span data-ttu-id="8076f-118">Para usar el Scaffolding de ASP.NET, debe tener:</span><span class="sxs-lookup"><span data-stu-id="8076f-118">To use ASP.NET Scaffolding, you must have:</span></span>

- <span data-ttu-id="8076f-119">Microsoft Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="8076f-119">Microsoft Visual Studio 2013</span></span>
- <span data-ttu-id="8076f-120">Herramientas de desarrollo Web (parte de la instalación predeterminada de Visual Studio 2013)</span><span class="sxs-lookup"><span data-stu-id="8076f-120">Web Developer Tools (part of default Visual Studio 2013 installation)</span></span>
- <span data-ttu-id="8076f-121">Marcos Web ASP.NET y herramientas 2013 (parte de la instalación predeterminada de Visual Studio 2013)</span><span class="sxs-lookup"><span data-stu-id="8076f-121">ASP.NET Web Frameworks and Tools 2013 (part of default Visual Studio 2013 installation)</span></span>

## <a name="add-a-scaffolded-item-to-mvc-or-web-api"></a><span data-ttu-id="8076f-122">Agregar un elemento con scaffold a MVC o Web API</span><span class="sxs-lookup"><span data-stu-id="8076f-122">Add a scaffolded item to MVC or Web API</span></span>

<span data-ttu-id="8076f-123">Para agregar una scaffold, haga clic en el proyecto o una carpeta dentro del proyecto y seleccione **agregar** – **nuevo elemento de scaffolding**, tal y como se muestra en la siguiente imagen.</span><span class="sxs-lookup"><span data-stu-id="8076f-123">To add a scaffold, right-click either the project or a folder within the project, and select **Add** – **New Scaffolded Item**, as shown in the following image.</span></span>

![Agregar elemento de scaffolding](aspnet-scaffolding-overview/_static/image1.png)

<span data-ttu-id="8076f-125">Desde el **agregar Scaffold** ventana, seleccione el tipo de scaffold a agregar.</span><span class="sxs-lookup"><span data-stu-id="8076f-125">From the **Add Scaffold** window, select the type of scaffold to add.</span></span>

![Seleccione el tipo de scaffold](aspnet-scaffolding-overview/_static/image2.png)

<span data-ttu-id="8076f-127">El **Agregar controlador** ventana le ofrece la oportunidad de seleccionar opciones para generar el controlador, incluso si desea usar las nuevas características asincrónicas de Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="8076f-127">The **Add Controller** window gives you the opportunity to select options for generating the controller, including whether you want to use the new async features from Entity Framework 6.</span></span>

![Agregar controlador](aspnet-scaffolding-overview/_static/image3.png)

<span data-ttu-id="8076f-129">Las clases pertinentes y las páginas se crean para su escenario.</span><span class="sxs-lookup"><span data-stu-id="8076f-129">The relevant classes and pages are created for your scenario.</span></span> <span data-ttu-id="8076f-130">Por ejemplo, la siguiente imagen muestra el controlador de MVC y las vistas que se crearon mediante scaffolding para una clase de modelo denominada películas.</span><span class="sxs-lookup"><span data-stu-id="8076f-130">For example, the following image shows the MVC controller and views that were created through scaffolding for a model class named Movies.</span></span>

![Los archivos creados](aspnet-scaffolding-overview/_static/image4.png)

## <a name="add-a-scaffolded-item-to-web-forms"></a><span data-ttu-id="8076f-132">Agregar un elemento con scaffolding para formularios Web Forms</span><span class="sxs-lookup"><span data-stu-id="8076f-132">Add a scaffolded item to Web Forms</span></span>

<span data-ttu-id="8076f-133">Para agregar scaffolding que genera el código de formularios Web Forms, debe instalar una extensión para Visual Studio, o bien agregar dependencias de MVC.</span><span class="sxs-lookup"><span data-stu-id="8076f-133">To add scaffolding that generates Web Forms code, you must either install an extension to Visual Studio or add MVC dependencies.</span></span> <span data-ttu-id="8076f-134">A continuación se muestran ambos enfoques, pero solo necesita realizar uno de estos enfoques.</span><span class="sxs-lookup"><span data-stu-id="8076f-134">Both approaches are shown below, but you only need to do one of these approaches.</span></span>

### <a name="web-forms-scaffolding-extension"></a><span data-ttu-id="8076f-135">Formularios Web Forms Scaffolding de extensión</span><span class="sxs-lookup"><span data-stu-id="8076f-135">Web Forms Scaffolding Extension</span></span>

<span data-ttu-id="8076f-136">Puede instalar una extensión de Visual Studio que le permiten usar la técnica scaffolding con un proyecto de formularios Web Forms.</span><span class="sxs-lookup"><span data-stu-id="8076f-136">You can install a Visual Studio extension that enable you to use scaffolding with a Web Forms project.</span></span> <span data-ttu-id="8076f-137">En Visual Studio, seleccione **herramientas** y, a continuación, **extensiones y actualizaciones**.</span><span class="sxs-lookup"><span data-stu-id="8076f-137">In Visual Studio, select **Tools** and then **Extensions and Updates**.</span></span> <span data-ttu-id="8076f-138">En este cuadro de diálogo Buscar en la Galería de Visual Studio para **Web Forms Scaffolding**.</span><span class="sxs-lookup"><span data-stu-id="8076f-138">From this dialog search the Visual Studio Gallery for **Web Forms Scaffolding**.</span></span>

![instalar los formularios web forms scaffolding](aspnet-scaffolding-overview/_static/image5.png)

<span data-ttu-id="8076f-140">Para obtener más información, consulte [Web Forms Scaffolding](https://go.microsoft.com/fwlink/p/?LinkId=396478).</span><span class="sxs-lookup"><span data-stu-id="8076f-140">For more information, see [Web Forms Scaffolding](https://go.microsoft.com/fwlink/p/?LinkId=396478).</span></span>

### <a name="mvc-dependencies"></a><span data-ttu-id="8076f-141">Dependencias de MVC</span><span class="sxs-lookup"><span data-stu-id="8076f-141">MVC Dependencies</span></span>

<span data-ttu-id="8076f-142">Para agregar dependencias de MVC, seleccione **agregar** - **nuevo elemento de scaffolding**.</span><span class="sxs-lookup"><span data-stu-id="8076f-142">To add MVC dependencies, select **Add** - **New Scaffolded Item**.</span></span> <span data-ttu-id="8076f-143">En la ventana Agregar Scaffold, seleccione **dependencias de MVC**, tal y como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="8076f-143">In the Add Scaffold window, select **MVC Dependencies**, as shown below.</span></span>

![Agregar dependencias de MVC](aspnet-scaffolding-overview/_static/image6.png)

<span data-ttu-id="8076f-145">Hay dos opciones para scaffolding de MVC; Mínimo y completa.</span><span class="sxs-lookup"><span data-stu-id="8076f-145">There are two options for scaffolding MVC; Minimal and Full.</span></span> <span data-ttu-id="8076f-146">Si selecciona mínima, sólo los paquetes de NuGet y referencias de ASP.NET MVC se agregan al proyecto.</span><span class="sxs-lookup"><span data-stu-id="8076f-146">If you select Minimal, only the NuGet packages and references for ASP.NET MVC are added to your project.</span></span> <span data-ttu-id="8076f-147">Si selecciona la opción completa, se agregan las dependencias mínimas, así como los archivos de contenido necesarios para un proyecto de MVC.</span><span class="sxs-lookup"><span data-stu-id="8076f-147">If you select the Full option, the Minimal dependencies are added, as well as the required content files for an MVC project.</span></span> <span data-ttu-id="8076f-148">Para utilizar fácilmente la técnica scaffolding, seleccione dependencias completas.</span><span class="sxs-lookup"><span data-stu-id="8076f-148">To easily use scaffolding, select Full dependencies.</span></span>

![Seleccione dependencias completas](aspnet-scaffolding-overview/_static/image7.png)

<span data-ttu-id="8076f-150">Después de agregar las dependencias, verá un **readme.txt** archivo.</span><span class="sxs-lookup"><span data-stu-id="8076f-150">After adding the dependencies, you will see a **readme.txt** file.</span></span> <span data-ttu-id="8076f-151">Siga cuidadosamente las instrucciones de este archivo para asegurarse de que el proyecto funcione correctamente.</span><span class="sxs-lookup"><span data-stu-id="8076f-151">Carefully follow the instructions in this file to ensure that your project works correctly.</span></span>

<span data-ttu-id="8076f-152">Cuando haya completado los pasos descritos en el archivo readme.txt, puede agregar un nuevo elemento con scaffolding, tal como se muestra en la sección anterior sobre MVC y Web API.</span><span class="sxs-lookup"><span data-stu-id="8076f-152">When you have completed the steps in the readme.txt file, you can add a new scaffolded item as shown in the previous section about MVC and Web API.</span></span> <span data-ttu-id="8076f-153">Las vistas de genera automáticamente y el controlador funcionará correctamente dentro del proyecto.</span><span class="sxs-lookup"><span data-stu-id="8076f-153">The automatically-generated views and controller will function correctly within your project.</span></span>

## <a name="tutorials"></a><span data-ttu-id="8076f-154">Tutoriales</span><span class="sxs-lookup"><span data-stu-id="8076f-154">Tutorials</span></span>

<span data-ttu-id="8076f-155">Para crear un proveedor de scaffolding personalizado, consulte [creación de un proveedor de scaffolding personalizado para Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).</span><span class="sxs-lookup"><span data-stu-id="8076f-155">To create a customized scaffolder, see [Creating a Custom Scaffolder for Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).</span></span>

<span data-ttu-id="8076f-156">Para personalizar los archivos generados, vea [cómo personalizar los archivos generados en el cuadro de diálogo nuevo elemento de scaffolding](https://blogs.msdn.com/b/webdev/archive/2013/12/26/how-to-customize-the-generated-files-from-the-new-scaffolded-item-dialog.aspx).</span><span class="sxs-lookup"><span data-stu-id="8076f-156">To customize the generated files, see [How to customize the generated files from the New Scaffolded Item dialog](https://blogs.msdn.com/b/webdev/archive/2013/12/26/how-to-customize-the-generated-files-from-the-new-scaffolded-item-dialog.aspx).</span></span>

<span data-ttu-id="8076f-157">Para obtener un ejemplo del uso de la técnica scaffolding con **desarrollo Database First**, consulte [EF Database First con ASP.NET MVC](../../../mvc/overview/getting-started/database-first-development/setting-up-database.md).</span><span class="sxs-lookup"><span data-stu-id="8076f-157">For an example of using scaffolding with **Database First development**, see [EF Database First with ASP.NET MVC](../../../mvc/overview/getting-started/database-first-development/setting-up-database.md).</span></span>

<span data-ttu-id="8076f-158">Para obtener un ejemplo del uso de la técnica scaffolding en un **MVC** de proyecto, consulte [Introducción a ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="8076f-158">For an example of using scaffolding in an **MVC** project, see [Getting Started with ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).</span></span>

<span data-ttu-id="8076f-159">Para obtener un ejemplo del uso de la técnica scaffolding en un **API Web** de proyecto, consulte [crear una API de REST con enrutamiento mediante atributos en Web API 2](../../../web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing.md).</span><span class="sxs-lookup"><span data-stu-id="8076f-159">For an example of using scaffolding in a **Web API** project, see [Create a REST API with Attribute Routing in Web API 2](../../../web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing.md).</span></span>
