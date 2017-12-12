---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-2
title: 'Parte 2: Capa de acceso a datos | Documentos de Microsoft'
author: JoeStagner
description: "Esta serie de tutoriales detalla todos los pasos realizados para compilar la aplicación de ejemplo de Tailspin Spyworks. Parte 2 describe cómo agregar la capa de acceso a datos."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 5a9d5429-d70b-411c-8474-f42cf7ef8a2b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-2
msc.type: authoredcontent
ms.openlocfilehash: 8b07b320640c1bb0074a4d3a04ca7c5b7e7bb6cd
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="part-2-data-access-layer"></a><span data-ttu-id="e1b7b-104">Parte 2: Capa de acceso a datos</span><span class="sxs-lookup"><span data-stu-id="e1b7b-104">Part 2: Data Access Layer</span></span>
====================
<span data-ttu-id="e1b7b-105">por [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="e1b7b-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="e1b7b-106">Tailspin Spyworks muestra cómo extraordinariamente simple es crear aplicaciones eficaces y escalables para la plataforma. NET.</span><span class="sxs-lookup"><span data-stu-id="e1b7b-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="e1b7b-107">Muestra cómo usar las características nuevas en ASP.NET 4 para crear una tienda en línea, incluidos los de la compra, la desprotección y la administración.</span><span class="sxs-lookup"><span data-stu-id="e1b7b-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="e1b7b-108">Esta serie de tutoriales detalla todos los pasos realizados para compilar la aplicación de ejemplo de Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="e1b7b-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="e1b7b-109">Parte 2 describe cómo agregar la capa de acceso a datos.</span><span class="sxs-lookup"><span data-stu-id="e1b7b-109">Part 2 covers adding the data access layer.</span></span>


## <a id="_Toc260221668"></a><span data-ttu-id="e1b7b-110">Agregar la capa de acceso a datos</span><span class="sxs-lookup"><span data-stu-id="e1b7b-110">Adding the Data Access Layer</span></span>

<span data-ttu-id="e1b7b-111">Nuestra aplicación de comercio electrónico dependerán de dos bases de datos.</span><span class="sxs-lookup"><span data-stu-id="e1b7b-111">Our ecommerce application will depend on two databases.</span></span>

<span data-ttu-id="e1b7b-112">Para obtener información de cliente, vamos a usar la base de datos estándar de pertenencia a ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="e1b7b-112">For customer information we'll use the standard ASP.NET Membership database.</span></span> <span data-ttu-id="e1b7b-113">Para nuestro catálogo de producto y el carro de la compra se implementará una base de datos de SQL Express como se indica a continuación.</span><span class="sxs-lookup"><span data-stu-id="e1b7b-113">For our shopping cart and product catalog we'll implement a SQL Express database as follows.</span></span>

![](tailspin-spyworks-part-2/_static/image1.jpg)

<span data-ttu-id="e1b7b-114">Al haber creado la base de datos (Commerce.mdf) en la aplicación de la aplicación\_carpeta de datos se puede empezar a crear la capa de acceso a datos utilizando la entidad de .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="e1b7b-114">Having created the database (Commerce.mdf) in the application's App\_Data folder we can proceed to create our Data Access Layer using the .NET Entity Framework.</span></span>

<span data-ttu-id="e1b7b-115">Vamos a crear una carpeta denominada "datos\_acceso" y haga clic con el botón secundario en esa carpeta y seleccione "Agregar nuevo elemento".</span><span class="sxs-lookup"><span data-stu-id="e1b7b-115">We'll create a folder named "Data\_Access" and them right click on that folder and select "Add New Item".</span></span>

<span data-ttu-id="e1b7b-116">En la "plantillas instaladas" elemento y, a continuación, seleccione "ADO.NET Entity Data Model" escriba EDM\_Commerce.edmx como el nombre y haga clic en el botón "Agregar".</span><span class="sxs-lookup"><span data-stu-id="e1b7b-116">In the "Installed Templates" item and then select "ADO.NET Entity Data Model" enter EDM\_Commerce.edmx as the name and click the "Add" button.</span></span>

![](tailspin-spyworks-part-2/_static/image2.jpg)

<span data-ttu-id="e1b7b-117">Elija "Generar desde la base de datos".</span><span class="sxs-lookup"><span data-stu-id="e1b7b-117">Choose "Generate from Database".</span></span>

![](tailspin-spyworks-part-2/_static/image1.png)

![](tailspin-spyworks-part-2/_static/image2.png)

![](tailspin-spyworks-part-2/_static/image3.png)

![](tailspin-spyworks-part-2/_static/image3.jpg)

<span data-ttu-id="e1b7b-118">Guarde y compile.</span><span class="sxs-lookup"><span data-stu-id="e1b7b-118">Save and build.</span></span>

<span data-ttu-id="e1b7b-119">Ahora estamos preparados para agregar la primera función – un menú de la categoría de producto.</span><span class="sxs-lookup"><span data-stu-id="e1b7b-119">Now we are ready to add our first feature – a product category menu.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="e1b7b-120">[Anterior](tailspin-spyworks-part-1.md)
[Siguiente](tailspin-spyworks-part-3.md)</span><span class="sxs-lookup"><span data-stu-id="e1b7b-120">[Previous](tailspin-spyworks-part-1.md)
[Next](tailspin-spyworks-part-3.md)</span></span>
