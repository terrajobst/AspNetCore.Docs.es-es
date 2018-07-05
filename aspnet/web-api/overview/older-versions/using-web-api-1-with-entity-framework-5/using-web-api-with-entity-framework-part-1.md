---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-1
title: 'Parte 1: Información general y creación del proyecto | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/03/2012
ms.topic: article
ms.assetid: 94421d86-68c4-4471-bf5f-82d654a17252
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-1
msc.type: authoredcontent
ms.openlocfilehash: f0616383fce2e92f7d1a0b63bf840208f7327bf7
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37394066"
---
<a name="part-1-overview-and-creating-the-project"></a><span data-ttu-id="67263-102">Parte 1: Información general y crear el proyecto</span><span class="sxs-lookup"><span data-stu-id="67263-102">Part 1: Overview and Creating the Project</span></span>
====================
<span data-ttu-id="67263-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="67263-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="67263-104">Descargue el proyecto completado</span><span class="sxs-lookup"><span data-stu-id="67263-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

<span data-ttu-id="67263-105">Entity Framework es un marco de asignación relacional de objetos.</span><span class="sxs-lookup"><span data-stu-id="67263-105">Entity Framework is an object/relational mapping framework.</span></span> <span data-ttu-id="67263-106">Los objetos de dominio en el código lo asigna a las entidades de una base de datos relacional.</span><span class="sxs-lookup"><span data-stu-id="67263-106">It maps the domain objects in your code to entities in a relational database.</span></span> <span data-ttu-id="67263-107">En su mayor parte, no es necesario preocuparse por la capa de base de datos, dado que Entity Framework se encarga de ello automáticamente.</span><span class="sxs-lookup"><span data-stu-id="67263-107">For the most part, you do not have to worry about the database layer, because Entity Framework takes care of it for you.</span></span> <span data-ttu-id="67263-108">El código manipula los objetos y los cambios se guardan en una base de datos.</span><span class="sxs-lookup"><span data-stu-id="67263-108">Your code manipulates the objects, and changes are persisted to a database.</span></span>

## <a name="about-the-tutorial"></a><span data-ttu-id="67263-109">Acerca del Tutorial</span><span class="sxs-lookup"><span data-stu-id="67263-109">About the Tutorial</span></span>

<span data-ttu-id="67263-110">En este tutorial, creará una aplicación de almacenamiento simple.</span><span class="sxs-lookup"><span data-stu-id="67263-110">In this tutorial, you will create a simple store application.</span></span> <span data-ttu-id="67263-111">Hay dos partes principales a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="67263-111">There are two main parts to the application.</span></span> <span data-ttu-id="67263-112">Los usuarios normales pueden ver los productos y crear pedidos:</span><span class="sxs-lookup"><span data-stu-id="67263-112">Normal users can view products and create orders:</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image1.png)

<span data-ttu-id="67263-113">Los administradores pueden crear, eliminar o modificar los productos:</span><span class="sxs-lookup"><span data-stu-id="67263-113">Administrators can create, delete, or edit products:</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image2.png)

## <a name="skills-youll-learn"></a><span data-ttu-id="67263-114">Habilidades que aprenderá</span><span class="sxs-lookup"><span data-stu-id="67263-114">Skills You'll Learn</span></span>

<span data-ttu-id="67263-115">Aquí es lo que aprenderá:</span><span class="sxs-lookup"><span data-stu-id="67263-115">Here's what you'll learn:</span></span>

- <span data-ttu-id="67263-116">Cómo usar Entity Framework con ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="67263-116">How to use Entity Framework with ASP.NET Web API.</span></span>
- <span data-ttu-id="67263-117">Cómo usar knockout.js para crear una interfaz de usuario del cliente dinámico.</span><span class="sxs-lookup"><span data-stu-id="67263-117">How to use knockout.js to create a dynamic client UI.</span></span>
- <span data-ttu-id="67263-118">Aprenda a utilizar autenticación de formularios con la API Web para autenticar a los usuarios.</span><span class="sxs-lookup"><span data-stu-id="67263-118">How to use forms authentication with Web API to authenticate users.</span></span>

<span data-ttu-id="67263-119">Aunque este tutorial es independiente, desee leer primero los siguientes tutoriales:</span><span class="sxs-lookup"><span data-stu-id="67263-119">Although this tutorial is self-contained, you might want to read the following tutorials first:</span></span>

- [<span data-ttu-id="67263-120">Su primer ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="67263-120">Your First ASP.NET Web API</span></span>](../../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)
- [<span data-ttu-id="67263-121">Crear una API Web que admita operaciones CRUD</span><span class="sxs-lookup"><span data-stu-id="67263-121">Creating a Web API that Supports CRUD Operations</span></span>](../creating-a-web-api-that-supports-crud-operations.md)

<span data-ttu-id="67263-122">Algunos conocimientos de [ASP.NET MVC](../../../../mvc/index.md) también es útil.</span><span class="sxs-lookup"><span data-stu-id="67263-122">Some knowledge of [ASP.NET MVC](../../../../mvc/index.md) is also helpful.</span></span>

## <a name="overview"></a><span data-ttu-id="67263-123">Información general</span><span class="sxs-lookup"><span data-stu-id="67263-123">Overview</span></span>

<span data-ttu-id="67263-124">En un nivel alto, ésta es la arquitectura de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="67263-124">At a high level, here is the architecture of the application:</span></span>

- <span data-ttu-id="67263-125">ASP.NET MVC genera las páginas HTML para el cliente.</span><span class="sxs-lookup"><span data-stu-id="67263-125">ASP.NET MVC generates the HTML pages for the client.</span></span>
- <span data-ttu-id="67263-126">ASP.NET Web API expone las operaciones CRUD en los datos (productos y pedidos).</span><span class="sxs-lookup"><span data-stu-id="67263-126">ASP.NET Web API exposes CRUD operations on the data (products and orders).</span></span>
- <span data-ttu-id="67263-127">Entity Framework traduce los modelos de C# usados por la API Web en las entidades de base de datos.</span><span class="sxs-lookup"><span data-stu-id="67263-127">Entity Framework translates the C# models used by Web API into database entities.</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image3.png)

<span data-ttu-id="67263-128">El diagrama siguiente muestra cómo se representan los objetos de dominio en varias capas de la aplicación: la capa de base de datos, el modelo de objetos y, finalmente, el formato, que se usa para transmitir datos al cliente a través de HTTP.</span><span class="sxs-lookup"><span data-stu-id="67263-128">The following diagram shows how the domain objects are represented at various layers of the application: The database layer, the object model, and finally the wire format, which is used to transmit data to the client via HTTP.</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image4.png)

## <a name="create-the-visual-studio-project"></a><span data-ttu-id="67263-129">Crear el proyecto de Visual Studio</span><span class="sxs-lookup"><span data-stu-id="67263-129">Create the Visual Studio Project</span></span>

<span data-ttu-id="67263-130">Puede crear el proyecto del tutorial con la versión completa de Visual Studio o Visual Web Developer Express.</span><span class="sxs-lookup"><span data-stu-id="67263-130">You can create the tutorial project using either Visual Web Developer Express or the full version of Visual Studio.</span></span>

<span data-ttu-id="67263-131">Desde el **iniciar** página, haga clic en **nuevo proyecto**.</span><span class="sxs-lookup"><span data-stu-id="67263-131">From the **Start** page, click **New Project**.</span></span>

<span data-ttu-id="67263-132">En el **plantillas** panel, seleccione **plantillas instaladas** y expanda el **Visual C#** nodo.</span><span class="sxs-lookup"><span data-stu-id="67263-132">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="67263-133">En **Visual C#**, seleccione **Web**.</span><span class="sxs-lookup"><span data-stu-id="67263-133">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="67263-134">En la lista de plantillas de proyecto, seleccione **aplicación Web de ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="67263-134">In the list of project templates, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="67263-135">Denomine el proyecto "ProductStore" y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="67263-135">Name the project "ProductStore" and click **OK**.</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image5.png)

<span data-ttu-id="67263-136">En el **nuevo proyecto de ASP.NET MVC 4** cuadro de diálogo, seleccione **aplicación de Internet** y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="67263-136">In the **New ASP.NET MVC 4 Project** dialog, select **Internet Application** and click **OK**.</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image6.png)

<span data-ttu-id="67263-137">La plantilla "Aplicación de Internet", crea una aplicación de ASP.NET MVC que admite la autenticación de formularios.</span><span class="sxs-lookup"><span data-stu-id="67263-137">The "Internet Application" template creates an ASP.NET MVC application that supports forms authentication.</span></span> <span data-ttu-id="67263-138">Si ejecuta la aplicación ahora, ya tiene algunas características:</span><span class="sxs-lookup"><span data-stu-id="67263-138">If you run the application now, it already has some features:</span></span>

- <span data-ttu-id="67263-139">Pueden registrar nuevos usuarios, haga clic en el vínculo "Register" en la esquina superior derecha.</span><span class="sxs-lookup"><span data-stu-id="67263-139">New users can register by clicking the "Register" link in the upper right corner.</span></span>
- <span data-ttu-id="67263-140">Los usuarios registrados pueden iniciar sesión, haga clic en el vínculo "Iniciar sesión".</span><span class="sxs-lookup"><span data-stu-id="67263-140">Registered users can log in by clicking the "Log in" link.</span></span>

<span data-ttu-id="67263-141">Información de pertenencia se conserva en una base de datos que se crea automáticamente.</span><span class="sxs-lookup"><span data-stu-id="67263-141">Membership information is persisted in a database that gets created automatically.</span></span> <span data-ttu-id="67263-142">Para obtener más información sobre la autenticación de formularios en ASP.NET MVC, consulte [Tutorial: uso de autenticación de formularios en ASP.NET MVC](https://msdn.microsoft.com/library/ff398049(VS.98).aspx).</span><span class="sxs-lookup"><span data-stu-id="67263-142">For more information about forms authentication in ASP.NET MVC, see [Walkthrough: Using Forms Authentication in ASP.NET MVC](https://msdn.microsoft.com/library/ff398049(VS.98).aspx).</span></span>

## <a name="update-the-css-file"></a><span data-ttu-id="67263-143">Actualice el archivo CSS</span><span class="sxs-lookup"><span data-stu-id="67263-143">Update the CSS File</span></span>

<span data-ttu-id="67263-144">Este paso es cosmético, pero hará que las páginas representará de esta forma las capturas de pantalla anteriores.</span><span class="sxs-lookup"><span data-stu-id="67263-144">This step is cosmetic, but it will make the pages render like the earlier screen shots.</span></span>

<span data-ttu-id="67263-145">En el Explorador de soluciones, expanda la carpeta de contenido y abra el archivo Site.css.</span><span class="sxs-lookup"><span data-stu-id="67263-145">In Solution Explorer, expand the Content folder and open the file named Site.css.</span></span> <span data-ttu-id="67263-146">Agregue los siguientes estilos CSS:</span><span class="sxs-lookup"><span data-stu-id="67263-146">Add the following CSS styles:</span></span>

[!code-css[Main](using-web-api-with-entity-framework-part-1/samples/sample1.css)]

> [!div class="step-by-step"]
> [<span data-ttu-id="67263-147">Siguiente</span><span class="sxs-lookup"><span data-stu-id="67263-147">Next</span></span>](using-web-api-with-entity-framework-part-2.md)
