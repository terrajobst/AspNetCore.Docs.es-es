---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-2
title: 'Parte 2: Capa de acceso a datos | Microsoft Docs'
author: JoeStagner
description: Esta serie de tutoriales detalla todos los pasos que se tarda en compilar la aplicación de ejemplo Tailspin Spyworks. Parte 2 describe cómo agregar la capa de acceso a datos.
ms.author: aspnetcontent
ms.date: 07/21/2010
ms.assetid: 5a9d5429-d70b-411c-8474-f42cf7ef8a2b
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-2
msc.type: authoredcontent
ms.openlocfilehash: c1d61681952cbf1f0587f0e42da21f8436d79ebb
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37812037"
---
<a name="part-2-data-access-layer"></a><span data-ttu-id="44fa9-104">Parte 2: Capa de acceso a datos</span><span class="sxs-lookup"><span data-stu-id="44fa9-104">Part 2: Data Access Layer</span></span>
====================
<span data-ttu-id="44fa9-105">por [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="44fa9-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="44fa9-106">Tailspin Spyworks demuestra cómo extraordinariamente simple es crear aplicaciones eficaces y escalables para la plataforma. NET.</span><span class="sxs-lookup"><span data-stu-id="44fa9-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="44fa9-107">Resalta cómo usar las características nuevas en ASP.NET 4 para crear una tienda en línea, incluida la compra, la desprotección y la administración.</span><span class="sxs-lookup"><span data-stu-id="44fa9-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="44fa9-108">Esta serie de tutoriales detalla todos los pasos que se tarda en compilar la aplicación de ejemplo Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="44fa9-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="44fa9-109">Parte 2 describe cómo agregar la capa de acceso a datos.</span><span class="sxs-lookup"><span data-stu-id="44fa9-109">Part 2 covers adding the data access layer.</span></span>


## <a id="_Toc260221668"></a>  <span data-ttu-id="44fa9-110">Adición de la capa de acceso a datos</span><span class="sxs-lookup"><span data-stu-id="44fa9-110">Adding the Data Access Layer</span></span>

<span data-ttu-id="44fa9-111">Nuestra aplicación de comercio electrónico dependerán de dos bases de datos.</span><span class="sxs-lookup"><span data-stu-id="44fa9-111">Our ecommerce application will depend on two databases.</span></span>

<span data-ttu-id="44fa9-112">Para obtener información de cliente, vamos a usar la base de datos de pertenencia ASP.NET estándar.</span><span class="sxs-lookup"><span data-stu-id="44fa9-112">For customer information we'll use the standard ASP.NET Membership database.</span></span> <span data-ttu-id="44fa9-113">Para nuestro catálogo de carro y producto compras implementaremos una base de datos de SQL Express como sigue.</span><span class="sxs-lookup"><span data-stu-id="44fa9-113">For our shopping cart and product catalog we'll implement a SQL Express database as follows.</span></span>

![](tailspin-spyworks-part-2/_static/image1.jpg)

<span data-ttu-id="44fa9-114">Haber creado la base de datos (Commerce.mdf) en la aplicación de la aplicación\_carpeta de datos, podemos continuar para crear la capa de acceso a datos mediante Entity Framework. NET.</span><span class="sxs-lookup"><span data-stu-id="44fa9-114">Having created the database (Commerce.mdf) in the application's App\_Data folder we can proceed to create our Data Access Layer using the .NET Entity Framework.</span></span>

<span data-ttu-id="44fa9-115">Vamos a crear una carpeta denominada "datos\_acceso" y haga clic con el botón derecho en esa carpeta y seleccione "Agregar nuevo elemento".</span><span class="sxs-lookup"><span data-stu-id="44fa9-115">We'll create a folder named "Data\_Access" and them right click on that folder and select "Add New Item".</span></span>

<span data-ttu-id="44fa9-116">En "Installed Templates" elemento y, a continuación, seleccione "ADO.NET Entity Data Model" escriba EDM\_Commerce.edmx como el nombre y haga clic en el botón "Agregar".</span><span class="sxs-lookup"><span data-stu-id="44fa9-116">In the "Installed Templates" item and then select "ADO.NET Entity Data Model" enter EDM\_Commerce.edmx as the name and click the "Add" button.</span></span>

![](tailspin-spyworks-part-2/_static/image2.jpg)

<span data-ttu-id="44fa9-117">Elija "Generar desde la base de datos".</span><span class="sxs-lookup"><span data-stu-id="44fa9-117">Choose "Generate from Database".</span></span>

![](tailspin-spyworks-part-2/_static/image1.png)

![](tailspin-spyworks-part-2/_static/image2.png)

![](tailspin-spyworks-part-2/_static/image3.png)

![](tailspin-spyworks-part-2/_static/image3.jpg)

<span data-ttu-id="44fa9-118">Guarde y compile.</span><span class="sxs-lookup"><span data-stu-id="44fa9-118">Save and build.</span></span>

<span data-ttu-id="44fa9-119">Ahora estamos listos para agregar la primera función: un menú de la categoría de producto.</span><span class="sxs-lookup"><span data-stu-id="44fa9-119">Now we are ready to add our first feature – a product category menu.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="44fa9-120">[Anterior](tailspin-spyworks-part-1.md)
> [Siguiente](tailspin-spyworks-part-3.md)</span><span class="sxs-lookup"><span data-stu-id="44fa9-120">[Previous](tailspin-spyworks-part-1.md)
[Next](tailspin-spyworks-part-3.md)</span></span>
