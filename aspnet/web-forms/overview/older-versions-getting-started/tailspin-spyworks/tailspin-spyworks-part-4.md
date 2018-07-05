---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-4
title: 'Parte 4: Lista de productos | Microsoft Docs'
author: JoeStagner
description: Esta serie de tutoriales detalla todos los pasos que se tarda en compilar la aplicación de ejemplo Tailspin Spyworks. Parte 4 cubre la lista de productos con el contr GridView...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 4fab47d5-a6ec-4fdc-91f0-651a093a24b9
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-4
msc.type: authoredcontent
ms.openlocfilehash: 929d88d747c25ca7c4f6f991421cb3aa9456aa45
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368814"
---
<a name="part-4-listing-products"></a><span data-ttu-id="63b9a-104">Parte 4: Lista de productos</span><span class="sxs-lookup"><span data-stu-id="63b9a-104">Part 4: Listing Products</span></span>
====================
<span data-ttu-id="63b9a-105">por [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="63b9a-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="63b9a-106">Tailspin Spyworks demuestra cómo extraordinariamente simple es crear aplicaciones eficaces y escalables para la plataforma. NET.</span><span class="sxs-lookup"><span data-stu-id="63b9a-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="63b9a-107">Resalta cómo usar las características nuevas en ASP.NET 4 para crear una tienda en línea, incluida la compra, la desprotección y la administración.</span><span class="sxs-lookup"><span data-stu-id="63b9a-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="63b9a-108">Esta serie de tutoriales detalla todos los pasos que se tarda en compilar la aplicación de ejemplo Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="63b9a-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="63b9a-109">Parte 4 trata la lista de productos con el control GridView.</span><span class="sxs-lookup"><span data-stu-id="63b9a-109">Part 4 covers listing products with the GridView control.</span></span>


## <a id="_Toc260221670"></a>  <span data-ttu-id="63b9a-110">Lista de productos con el Control GridView</span><span class="sxs-lookup"><span data-stu-id="63b9a-110">Listing Products with the GridView Control</span></span>

<span data-ttu-id="63b9a-111">Vamos a empezar a implementar nuestra página ProductsList.aspx haciendo clic en"secundario" en nuestra solución y seleccione "Agregar" y "Nuevo elemento".</span><span class="sxs-lookup"><span data-stu-id="63b9a-111">Let's begin implementing our ProductsList.aspx page by "Right Clicking" on our solution and selecting "Add" and "New Item".</span></span>

![](tailspin-spyworks-part-4/_static/image1.jpg)

<span data-ttu-id="63b9a-112">Elija "Uso de maestro de página de formularios Web" y escriba un nombre de la página de ProductsList.aspx".</span><span class="sxs-lookup"><span data-stu-id="63b9a-112">Choose "Web Form Using Master Page" and enter a page name of ProductsList.aspx".</span></span>

<span data-ttu-id="63b9a-113">Haga clic en "Agregar".</span><span class="sxs-lookup"><span data-stu-id="63b9a-113">Click "Add".</span></span>

![](tailspin-spyworks-part-4/_static/image2.jpg)

<span data-ttu-id="63b9a-114">A continuación, elija la carpeta "Estilos" donde se coloca la página Site.Master y selecciónelo en la ventana "Contenido de la carpeta".</span><span class="sxs-lookup"><span data-stu-id="63b9a-114">Next choose the "Styles" folder where we placed the Site.Master page and select it from the "Contents of folder" window.</span></span>

![](tailspin-spyworks-part-4/_static/image3.jpg)

<span data-ttu-id="63b9a-115">Haga clic en "Aceptar" para crear la página.</span><span class="sxs-lookup"><span data-stu-id="63b9a-115">Click "Ok" to create the page.</span></span>

<span data-ttu-id="63b9a-116">Nuestra base de datos se rellena con datos del producto tal como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="63b9a-116">Our database is populated with product data as seen below.</span></span>

![](tailspin-spyworks-part-4/_static/image4.jpg)

<span data-ttu-id="63b9a-117">Después de crear la página nuevo vamos a usar un origen de datos de entidad para acceder a los datos del producto, pero en esta instancia se debe seleccionar las entidades Product y necesitamos restringir los elementos que se devuelven a aquellos para la categoría seleccionada.</span><span class="sxs-lookup"><span data-stu-id="63b9a-117">After our page is created we'll again use an Entity Data Source to access that product data, but in this instance we need to select the Product Entities and we need to restrict the items that are returned to only those for the selected Category.</span></span>

<span data-ttu-id="63b9a-118">Para realizar esta acción se lo indicaremos EntityDataSource para generar automáticamente la cláusula WHERE y especificamos el WhereParameter.</span><span class="sxs-lookup"><span data-stu-id="63b9a-118">To accomplish this we'll tell the EntityDataSource to Auto Generate the WHERE clause and we'll specify the WhereParameter.</span></span>

<span data-ttu-id="63b9a-119">Recordará que cuando creamos los elementos de menú en nuestra "Menu de categoría de producto" se compila dinámicamente el enlace agregando el CatagoryID a la cadena de consulta para cada vínculo.</span><span class="sxs-lookup"><span data-stu-id="63b9a-119">You'll recall that when we created the Menu Items in our "Product Category Menu" we dynamically built the link by adding the CatagoryID to the QueryString for each link.</span></span> <span data-ttu-id="63b9a-120">Se le indicará que el origen de datos de entidad para el parámetro WHERE se derivan de ese parámetro de cadena de consulta.</span><span class="sxs-lookup"><span data-stu-id="63b9a-120">We will tell the Entity Data Source to derive the WHERE parameter from that QueryString parameter.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample1.aspx)]

<span data-ttu-id="63b9a-121">A continuación, vamos a configurar el control ListView para mostrar una lista de productos.</span><span class="sxs-lookup"><span data-stu-id="63b9a-121">Next, we'll configure the ListView control to display a list of products.</span></span> <span data-ttu-id="63b9a-122">Para crear una experiencia óptima de la compra que se deberá compactar varias características concisas en cada producto individual que se muestran en nuestra ListVew.</span><span class="sxs-lookup"><span data-stu-id="63b9a-122">To create an optimal shopping experience we'll compact several concise features into each individual product displayed in our ListVew.</span></span>

- <span data-ttu-id="63b9a-123">El nombre del producto será un vínculo a la vista de detalles del producto.</span><span class="sxs-lookup"><span data-stu-id="63b9a-123">The product name will be a link to the product's detail view.</span></span>
- <span data-ttu-id="63b9a-124">Se mostrará el precio del producto.</span><span class="sxs-lookup"><span data-stu-id="63b9a-124">The product's price will be displayed.</span></span>
- <span data-ttu-id="63b9a-125">Se mostrará una imagen del producto y dinámicamente seleccionaremos la imagen desde un directorio de imágenes del catálogo en nuestra aplicación.</span><span class="sxs-lookup"><span data-stu-id="63b9a-125">An image of the product will be displayed and we'll dynamically select the image from a catalog images directory in our application.</span></span>
- <span data-ttu-id="63b9a-126">Se incluirá un vínculo para agregar el producto específico al carro de la compra de inmediato.</span><span class="sxs-lookup"><span data-stu-id="63b9a-126">We will include a link to immediately add the specific product to the shopping cart.</span></span>

<span data-ttu-id="63b9a-127">Aquí está el marcado de nuestra instancia del control ListView.</span><span class="sxs-lookup"><span data-stu-id="63b9a-127">Here is the markup for our ListView control instance.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample2.aspx)]

<span data-ttu-id="63b9a-128">Estamos creando varios vínculos para cada producto muestra dinámicamente.</span><span class="sxs-lookup"><span data-stu-id="63b9a-128">We are dynamically building several links for each displayed product.</span></span>

<span data-ttu-id="63b9a-129">Además, antes de que probamos la propia página nueva debemos crear la estructura de directorios para el producto imágenes del catálogo como sigue.</span><span class="sxs-lookup"><span data-stu-id="63b9a-129">Also, before we test own new page we need to create the directory structure for the product catalog images as follows.</span></span>

![](tailspin-spyworks-part-4/_static/image1.png)

<span data-ttu-id="63b9a-130">Una vez que nuestras imágenes de producto son accesibles podemos probar la página de lista del producto.</span><span class="sxs-lookup"><span data-stu-id="63b9a-130">Once our product images are accessible we can test our product list page.</span></span>

![](tailspin-spyworks-part-4/_static/image5.jpg)

<span data-ttu-id="63b9a-131">En la página del sitio principal, haga clic en uno de los vínculos de categoría de lista.</span><span class="sxs-lookup"><span data-stu-id="63b9a-131">From the site's home page, click on one of the Category List Links.</span></span>

![](tailspin-spyworks-part-4/_static/image6.jpg)

<span data-ttu-id="63b9a-132">Ahora debemos implementar la página ProductDetials.apsx y la funcionalidad de AddToCart.</span><span class="sxs-lookup"><span data-stu-id="63b9a-132">Now we need to implement the ProductDetials.apsx page and the AddToCart functionality.</span></span>

<span data-ttu-id="63b9a-133">Use archivo -&gt;nuevo para crear un nombre de página ProductDetails.aspx mediante la página principal del sitio como hicimos anteriormente.</span><span class="sxs-lookup"><span data-stu-id="63b9a-133">Use File-&gt;New to create a page name ProductDetails.aspx using the site Master Page as we did previously.</span></span>

<span data-ttu-id="63b9a-134">Nuevamente, usaremos un control EntityDataSource para acceder al registro de producto específico en la base de datos y se usará un control FormView de ASP.NET para mostrar los datos de productos como sigue.</span><span class="sxs-lookup"><span data-stu-id="63b9a-134">We will again use an EntityDataSource control to access the specific product record in the database and we will use an ASP.NET FormView control to display the product data as follows.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample3.aspx)]

<span data-ttu-id="63b9a-135">No se preocupe si el formato se ve algo divertido para usted.</span><span class="sxs-lookup"><span data-stu-id="63b9a-135">Don't worry if the formatting looks a bit funny to you.</span></span> <span data-ttu-id="63b9a-136">El marcado anterior deja espacio en el diseño de presentación para un par de características que se implementará más adelante.</span><span class="sxs-lookup"><span data-stu-id="63b9a-136">The markup above leaves room in the display layout for a couple of features we'll implement later on.</span></span>

<span data-ttu-id="63b9a-137">El carro de la compra representará una lógica más compleja en nuestra aplicación.</span><span class="sxs-lookup"><span data-stu-id="63b9a-137">The Shopping Cart will represent the more complex logic in our application.</span></span> <span data-ttu-id="63b9a-138">Para empezar, use archivo -&gt;nuevo para crear una página denominada MyShoppingCart.aspx.</span><span class="sxs-lookup"><span data-stu-id="63b9a-138">To get started, use File-&gt;New to create a page called MyShoppingCart.aspx.</span></span>

<span data-ttu-id="63b9a-139">Tenga en cuenta que no nos estamos elección del nombre ShoppingCart.aspx.</span><span class="sxs-lookup"><span data-stu-id="63b9a-139">Note that we are not choosing the name ShoppingCart.aspx.</span></span>

<span data-ttu-id="63b9a-140">Nuestra base de datos contiene una tabla denominada "ShoppingCart".</span><span class="sxs-lookup"><span data-stu-id="63b9a-140">Our database contains a table named "ShoppingCart".</span></span> <span data-ttu-id="63b9a-141">Cuando se genera un Entity Data Model se crea una clase para cada tabla en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="63b9a-141">When we generated an Entity Data Model a class was created for each table in the database.</span></span> <span data-ttu-id="63b9a-142">Por lo tanto, el Entity Data Model genera una clase de entidad denominada "ShoppingCart".</span><span class="sxs-lookup"><span data-stu-id="63b9a-142">Therefore, the Entity Data Model generated an Entity Class named "ShoppingCart".</span></span> <span data-ttu-id="63b9a-143">Por lo que nos podríamos usar ese nombre para la implementación de carro de la compra o ampliarlo para nuestras necesidades, pero se elegirá en su lugar para simplemente seleccione un nombre que evitará el conflicto, se podríamos modificar el modelo.</span><span class="sxs-lookup"><span data-stu-id="63b9a-143">We could edit the model so that we could use that name for our shopping cart implementation or extend it for our needs, but we will opt instead to simply slect a name that will avoid the conflict.</span></span>

<span data-ttu-id="63b9a-144">También merece la pena tener en cuenta que se va a crear un carro de compra simple e insertar la lógica del carro de la compra con la presentación del carro de la compra.</span><span class="sxs-lookup"><span data-stu-id="63b9a-144">It's also worth noting that we will be creating a simple shopping cart and embedding the shopping cart logic with the shopping cart display.</span></span> <span data-ttu-id="63b9a-145">También podríamos optar por implementar nuestro carro de compra en una capa de negocio completamente independiente.</span><span class="sxs-lookup"><span data-stu-id="63b9a-145">We might also choose to implement our shopping cart in a completely separate Business Layer.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="63b9a-146">[Anterior](tailspin-spyworks-part-3.md)
> [Siguiente](tailspin-spyworks-part-5.md)</span><span class="sxs-lookup"><span data-stu-id="63b9a-146">[Previous](tailspin-spyworks-part-3.md)
[Next](tailspin-spyworks-part-5.md)</span></span>
