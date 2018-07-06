---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-3
title: 'Parte 3: Diseño y menú categoría | Microsoft Docs'
author: JoeStagner
description: Esta serie de tutoriales detalla todos los pasos que se tarda en compilar la aplicación de ejemplo Tailspin Spyworks. Parte 3 aborda la adición de diseño y un menú de la categoría.
ms.author: aspnetcontent
ms.date: 07/21/2010
ms.assetid: 94ea1a70-a9bc-4241-8f36-08366d64bab9
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-3
msc.type: authoredcontent
ms.openlocfilehash: 3047c53e21a418aef8617bd772a247bc46edb98f
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37817530"
---
<a name="part-3-layout-and-category-menu"></a><span data-ttu-id="6bd6e-104">Parte 3: Diseño y menú categoría</span><span class="sxs-lookup"><span data-stu-id="6bd6e-104">Part 3: Layout and Category Menu</span></span>
====================
<span data-ttu-id="6bd6e-105">por [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="6bd6e-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="6bd6e-106">Tailspin Spyworks demuestra cómo extraordinariamente simple es crear aplicaciones eficaces y escalables para la plataforma. NET.</span><span class="sxs-lookup"><span data-stu-id="6bd6e-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="6bd6e-107">Resalta cómo usar las características nuevas en ASP.NET 4 para crear una tienda en línea, incluida la compra, la desprotección y la administración.</span><span class="sxs-lookup"><span data-stu-id="6bd6e-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="6bd6e-108">Esta serie de tutoriales detalla todos los pasos que se tarda en compilar la aplicación de ejemplo Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="6bd6e-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="6bd6e-109">Parte 3 aborda la adición de diseño y un menú de la categoría.</span><span class="sxs-lookup"><span data-stu-id="6bd6e-109">Part 3 covers adding layout and a category menu.</span></span>


## <a id="_Toc260221669"></a>  <span data-ttu-id="6bd6e-110">Adición de algunos diseño y un menú categoría</span><span class="sxs-lookup"><span data-stu-id="6bd6e-110">Adding Some Layout and a Category Menu</span></span>

<span data-ttu-id="6bd6e-111">En la página principal del sitio vamos a agregar un elemento div para la columna del lado izquierdo que contendrá el menú de la categoría de producto.</span><span class="sxs-lookup"><span data-stu-id="6bd6e-111">In our site master page we'll add a div for the left side column that will contain our product category menu.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-3/samples/sample1.aspx)]

<span data-ttu-id="6bd6e-112">Tenga en cuenta que se proporcionará la alineación deseada y otro formato de la clase CSS que agregamos a nuestro archivo Style.css.</span><span class="sxs-lookup"><span data-stu-id="6bd6e-112">Note that the desired aligning and other formatting will be provided by the CSS class that we added to our Style.css file.</span></span>

[!code-css[Main](tailspin-spyworks-part-3/samples/sample2.css)]

<span data-ttu-id="6bd6e-113">El menú de la categoría de producto se crearán dinámicamente en tiempo de ejecución consultando la base de datos de comercio de categorías de producto existente y crear los elementos de menú y correspondientes vínculos.</span><span class="sxs-lookup"><span data-stu-id="6bd6e-113">The product category menu will be dynamically created at runtime by querying the Commerce database for existing product categories and creating the menu items and corresponding links.</span></span>

<span data-ttu-id="6bd6e-114">Para ello usaremos dos de ASP. Controles de datos eficaces de la red.</span><span class="sxs-lookup"><span data-stu-id="6bd6e-114">To accomplish this we will use two of ASP.NET's powerful data controls.</span></span> <span data-ttu-id="6bd6e-115">El control de "Origen de datos de entidad" y el control "ListView".</span><span class="sxs-lookup"><span data-stu-id="6bd6e-115">The "Entity Data Source" control and the "ListView" control.</span></span>

![](tailspin-spyworks-part-3/_static/image1.jpg)

<span data-ttu-id="6bd6e-116">Vamos a cambiar a la "Vista de diseño" y usar las aplicaciones auxiliares para configurar los controles.</span><span class="sxs-lookup"><span data-stu-id="6bd6e-116">Let's switch to "Design View" and use the helpers to configure our controls.</span></span>

![](tailspin-spyworks-part-3/_static/image2.jpg)

<span data-ttu-id="6bd6e-117">Vamos a establecer la propiedad ID de EntityDataSource a EDS\_categoría\_menú y haga clic en "Configurar origen de datos".</span><span class="sxs-lookup"><span data-stu-id="6bd6e-117">Let's set the EntityDataSource ID property to EDS\_Category\_Menu and click on "Configure Data Source".</span></span>

![](tailspin-spyworks-part-3/_static/image3.jpg)

<span data-ttu-id="6bd6e-118">Seleccione la conexión de CommerceEntities que se creó para nosotros cuando creamos el modelo de origen de datos de entidad para nuestra base de datos de comercio y haga clic en "Siguiente".</span><span class="sxs-lookup"><span data-stu-id="6bd6e-118">Select the CommerceEntities Connection that was created for us when we created the Entity Data Source Model for our Commerce Database and click "Next".</span></span>

![](tailspin-spyworks-part-3/_static/image4.jpg)

<span data-ttu-id="6bd6e-119">Seleccione el nombre del conjunto de entidades de "Categorías" y deje el resto de las opciones como valor predeterminado.</span><span class="sxs-lookup"><span data-stu-id="6bd6e-119">Select the "Categories" Entity set name and leave the rest of the options as default.</span></span> <span data-ttu-id="6bd6e-120">Haga clic en "Finalizar".</span><span class="sxs-lookup"><span data-stu-id="6bd6e-120">Click "Finish".</span></span>

<span data-ttu-id="6bd6e-121">Ahora vamos a establecer la propiedad ID de la instancia del control ListView que se coloca en nuestra página de ListView\_ProductsMenu y activar su aplicación auxiliar.</span><span class="sxs-lookup"><span data-stu-id="6bd6e-121">Now let's set the ID property of the ListView control instance that we placed on our page to ListView\_ProductsMenu and activate its helper.</span></span>

![](tailspin-spyworks-part-3/_static/image5.jpg)

<span data-ttu-id="6bd6e-122">Aunque podríamos usar las opciones de control para dar formato a la presentación del elemento de datos y formato, nuestra creación de menús sólo requerirá marcado simple por lo que se especificará el código en la vista del origen.</span><span class="sxs-lookup"><span data-stu-id="6bd6e-122">Though we could use control options to format the data item display and formatting, our menu creation will only require simple markup so we will enter the code in the source view.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-3/samples/sample3.aspx)]

<span data-ttu-id="6bd6e-123">Tenga en cuenta la instrucción "Eval": &lt;% # Eval("CategoryName") %&gt;</span><span class="sxs-lookup"><span data-stu-id="6bd6e-123">Please note the "Eval" statement : &lt;%# Eval("CategoryName") %&gt;</span></span>

<span data-ttu-id="6bd6e-124">La sintaxis ASP.NET &lt;% # %&gt; es una convención taquigrafía que indica el tiempo de ejecución para ejecutar todo lo que está dentro y mostrar los resultados "en línea".</span><span class="sxs-lookup"><span data-stu-id="6bd6e-124">The ASP.NET syntax &lt;%# %&gt; is a shorthand convention that instructs the runtime to execute whatever is contained within and output the results "in Line".</span></span>

<span data-ttu-id="6bd6e-125">La instrucción Eval("CategoryName") indica, a la entrada actual en la colección enlazada de elementos de datos, capturar el valor de los nombres de elemento de modelo de entidad "CatagoryName".</span><span class="sxs-lookup"><span data-stu-id="6bd6e-125">The statement Eval("CategoryName") instructs that, for the current entry in the bound collection of data items, fetch the value of the Entity Model item names "CatagoryName".</span></span> <span data-ttu-id="6bd6e-126">Se trata de una sintaxis concisa para una característica muy eficaz.</span><span class="sxs-lookup"><span data-stu-id="6bd6e-126">This is concise syntax for a very powerful feature.</span></span>

<span data-ttu-id="6bd6e-127">Permite ejecutar la aplicación ahora.</span><span class="sxs-lookup"><span data-stu-id="6bd6e-127">Lets run the application now.</span></span>

![](tailspin-spyworks-part-3/_static/image6.jpg)

<span data-ttu-id="6bd6e-128">Tenga en cuenta que ahora se muestra el menú de la categoría de producto y cuando se mantiene el puntero sobre uno de los elementos de menú categoría apunta el vínculo de elemento de menú a una página que se deben implementar con aún podemos ver denominado ProductsList.aspx y que hemos creado un argumento de cadena de consulta dinámica que contiene el  Id. de categoría.</span><span class="sxs-lookup"><span data-stu-id="6bd6e-128">Note that our product category menu is now displayed and when we hover over one of the category menu items we can see the menu item link points to a page we have yet to implement named ProductsList.aspx and that we have built a dynamic query string argument that contains the category id.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="6bd6e-129">[Anterior](tailspin-spyworks-part-2.md)
> [Siguiente](tailspin-spyworks-part-4.md)</span><span class="sxs-lookup"><span data-stu-id="6bd6e-129">[Previous](tailspin-spyworks-part-2.md)
[Next](tailspin-spyworks-part-4.md)</span></span>
