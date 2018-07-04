---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
title: 'Parte 7: Incorporación de características | Microsoft Docs'
author: JoeStagner
description: Esta serie de tutoriales detalla todos los pasos que se tarda en compilar la aplicación de ejemplo Tailspin Spyworks. Parte 7 agrega características adicionales, como revisa cuenta...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 50223ee9-11b9-4cf3-bca2-e2f10bf471f3
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
msc.type: authoredcontent
ms.openlocfilehash: 8cdde10981835877e5ac2f65860010920a68d0a2
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37389181"
---
<a name="part-7-adding-features"></a><span data-ttu-id="68d6c-104">Parte 7: Agregar características</span><span class="sxs-lookup"><span data-stu-id="68d6c-104">Part 7: Adding Features</span></span>
====================
<span data-ttu-id="68d6c-105">por [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="68d6c-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="68d6c-106">Tailspin Spyworks demuestra cómo extraordinariamente simple es crear aplicaciones eficaces y escalables para la plataforma. NET.</span><span class="sxs-lookup"><span data-stu-id="68d6c-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="68d6c-107">Resalta cómo usar las características nuevas en ASP.NET 4 para crear una tienda en línea, incluida la compra, la desprotección y la administración.</span><span class="sxs-lookup"><span data-stu-id="68d6c-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="68d6c-108">Esta serie de tutoriales detalla todos los pasos que se tarda en compilar la aplicación de ejemplo Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="68d6c-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="68d6c-109">Parte 7 agrega características adicionales, como la revisión de la cuenta, reseñas de productos y "elementos populares" y los controles de usuario "también adquiridas".</span><span class="sxs-lookup"><span data-stu-id="68d6c-109">Part 7 adds additional features, such as account review, product reviews, and "popular items" and "also purchased" user controls.</span></span>


## <a id="_Toc260221673"></a>  <span data-ttu-id="68d6c-110">Agregar características</span><span class="sxs-lookup"><span data-stu-id="68d6c-110">Adding Features</span></span>

<span data-ttu-id="68d6c-111">Aunque los usuarios pueden examinar nuestro catálogo, colocar elementos en su carro de la compra y completar el proceso de formalización, hay que un número de admitir las características que se incluirá para mejorar nuestro sitio.</span><span class="sxs-lookup"><span data-stu-id="68d6c-111">Though users can browse our catalog, place items in their shopping cart, and complete the checkout process, there are a number of supporting features that we will include to improve our site.</span></span>

1. <span data-ttu-id="68d6c-112">Revisión de la cuenta (lista de pedidos colocaron y ver los detalles.)</span><span class="sxs-lookup"><span data-stu-id="68d6c-112">Account Review (List orders placed and view details.)</span></span>
2. <span data-ttu-id="68d6c-113">Agregar algún contenido específico de contexto a la página inicial.</span><span class="sxs-lookup"><span data-stu-id="68d6c-113">Add some context specific content to the front page.</span></span>
3. <span data-ttu-id="68d6c-114">Agregar una característica para permitir que los usuarios de revisión de los productos en el catálogo.</span><span class="sxs-lookup"><span data-stu-id="68d6c-114">Add a feature to let users Review the products in the catalog.</span></span>
4. <span data-ttu-id="68d6c-115">Crear un Control de usuario para mostrar elementos populares y contexto que controlan en la página principal.</span><span class="sxs-lookup"><span data-stu-id="68d6c-115">Create a User Control to display Popular Items and Place that control on the front page.</span></span>
5. <span data-ttu-id="68d6c-116">Crear un control de usuario "También adquirido" y agregarlo a la página de detalles del producto.</span><span class="sxs-lookup"><span data-stu-id="68d6c-116">Create an "Also Purchased" user control and add it to the product details page.</span></span>
6. <span data-ttu-id="68d6c-117">Agregar un contacto de página.</span><span class="sxs-lookup"><span data-stu-id="68d6c-117">Add a Contact Page.</span></span>
7. <span data-ttu-id="68d6c-118">Agregar una página.</span><span class="sxs-lookup"><span data-stu-id="68d6c-118">Add an About Page.</span></span>
8. <span data-ttu-id="68d6c-119">Error global</span><span class="sxs-lookup"><span data-stu-id="68d6c-119">Global Error</span></span>

## <a id="_Toc260221674"></a>  <span data-ttu-id="68d6c-120">Revisión de la cuenta</span><span class="sxs-lookup"><span data-stu-id="68d6c-120">Account Review</span></span>

<span data-ttu-id="68d6c-121">En la carpeta de "Cuenta", cree dos páginas .aspx uno OrderList.aspx con nombre y el otro OrderDetails.aspx con nombre</span><span class="sxs-lookup"><span data-stu-id="68d6c-121">In the "Account" folder create two .aspx pages one named OrderList.aspx and the other named OrderDetails.aspx</span></span>

<span data-ttu-id="68d6c-122">OrderList.aspx aprovechará los controles GridView y EntityDataSoure mucho que tenemos anteriormente.</span><span class="sxs-lookup"><span data-stu-id="68d6c-122">OrderList.aspx will leverage the GridView and EntityDataSoure controls much as we have previously.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample1.aspx)]

<span data-ttu-id="68d6c-123">El EntityDataSoure selecciona los registros de la tabla de pedidos filtrada por el nombre de usuario (consulte la WhereParameter) que se establece en una variable de sesión cuando el registro de usuario.</span><span class="sxs-lookup"><span data-stu-id="68d6c-123">The EntityDataSoure selects records from the Orders table filtered on the UserName (see the WhereParameter) which we set in a session variable when the user log's in.</span></span>

<span data-ttu-id="68d6c-124">Tenga en cuenta también estos parámetros en el campo Hyperlink del control GridView:</span><span class="sxs-lookup"><span data-stu-id="68d6c-124">Note also these parameters in the HyperlinkField of the GridView:</span></span>

[!code-xml[Main](tailspin-spyworks-part-7/samples/sample2.xml)]

<span data-ttu-id="68d6c-125">Estos especifican que el vínculo a la vista de detalles de pedido para cada producto especificando el campo OrderID como un parámetro de cadena de consulta a la página OrderDetails.aspx.</span><span class="sxs-lookup"><span data-stu-id="68d6c-125">These specify the link to the Order details view for each product specifying the OrderID field as a QueryString parameter to the OrderDetails.aspx page.</span></span>

## <a id="_Toc260221675"></a>  <span data-ttu-id="68d6c-126">OrderDetails.aspx</span><span class="sxs-lookup"><span data-stu-id="68d6c-126">OrderDetails.aspx</span></span>

<span data-ttu-id="68d6c-127">Usaremos un control EntityDataSource para tener acceso a los pedidos y un FormView para mostrar los datos del pedido y otro EntityDataSource con un control GridView para mostrar los elementos de línea de todos los pedidos.</span><span class="sxs-lookup"><span data-stu-id="68d6c-127">We will use an EntityDataSource control to access the Orders and a FormView to display the Order data and another EntityDataSource with a GridView to display all the Order's line items.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample3.aspx)]

<span data-ttu-id="68d6c-128">En el archivo (OrderDetails.aspx.cs) de código subyacente, tenemos dos bits poco ciertos aspectos de mantenimiento.</span><span class="sxs-lookup"><span data-stu-id="68d6c-128">In the Code Behind file (OrderDetails.aspx.cs) we have two little bits of housekeeping.</span></span>

<span data-ttu-id="68d6c-129">En primer lugar, debe asegurarse de que OrderDetails siempre obtiene un OrderId.</span><span class="sxs-lookup"><span data-stu-id="68d6c-129">First we need to make sure that OrderDetails always gets an OrderId.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample4.cs)]

<span data-ttu-id="68d6c-130">También es necesario calcular y mostrar el total de los elementos de línea del pedido.</span><span class="sxs-lookup"><span data-stu-id="68d6c-130">We also need to calculate and display the order total from the line items.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample5.cs)]

## <a id="_Toc260221676"></a>  <span data-ttu-id="68d6c-131">La página principal</span><span class="sxs-lookup"><span data-stu-id="68d6c-131">The Home Page</span></span>

<span data-ttu-id="68d6c-132">Vamos a agregar algún contenido estático a la página Default.aspx.</span><span class="sxs-lookup"><span data-stu-id="68d6c-132">Let's add some static content to the Default.aspx page.</span></span>

<span data-ttu-id="68d6c-133">Primero crearé una carpeta de "Contenido" y dentro de él una carpeta de imágenes (y deberá incluir una imagen que se usará en la página principal).</span><span class="sxs-lookup"><span data-stu-id="68d6c-133">First I'll create a "Content" folder and within it an Images folder (and I'll include an image to be used on the home page.)</span></span>

<span data-ttu-id="68d6c-134">En el marcador de posición de la parte inferior de la página Default.aspx, agregue el marcado siguiente.</span><span class="sxs-lookup"><span data-stu-id="68d6c-134">Into the bottom placeholder of the Default.aspx page, add the following markup.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample6.aspx)]

## <a id="_Toc260221677"></a>  <span data-ttu-id="68d6c-135">Reseñas de productos</span><span class="sxs-lookup"><span data-stu-id="68d6c-135">Product Reviews</span></span>

<span data-ttu-id="68d6c-136">En primer lugar, vamos a agregar un botón con un vínculo a un formulario que podemos usar para escribir una reseña del producto.</span><span class="sxs-lookup"><span data-stu-id="68d6c-136">First we'll add a button with a link to a form that we can use to enter a product review.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample7.aspx)]

![](tailspin-spyworks-part-7/_static/image1.jpg)

<span data-ttu-id="68d6c-137">Tenga en cuenta que el ProductID que estamos pasando la cadena de consulta</span><span class="sxs-lookup"><span data-stu-id="68d6c-137">Note that we are passing the ProductID in the query string</span></span>

<span data-ttu-id="68d6c-138">Siguiente vamos a agregar página denominada ReviewAdd.aspx</span><span class="sxs-lookup"><span data-stu-id="68d6c-138">Next let's add page named ReviewAdd.aspx</span></span>

<span data-ttu-id="68d6c-139">Esta página utilizará ASP.NET AJAX Control Toolkit.</span><span class="sxs-lookup"><span data-stu-id="68d6c-139">This page will use the ASP.NET AJAX Control Toolkit.</span></span> <span data-ttu-id="68d6c-140">Si aún no ha hecho por lo que puede descargar desde [DevExpress](http://devexpress.com/act) y hay una guía sobre cómo configurar el Kit de herramientas para su uso con Visual Studio aquí [ https://www.asp.net/learn/ajax-videos/video-76.aspx ](../../../videos/ajax-control-toolkit/how-do-i-get-started-with-the-aspnet-ajax-control-toolkit.md).</span><span class="sxs-lookup"><span data-stu-id="68d6c-140">If you have not already done so you can download it from [DevExpress](http://devexpress.com/act) and there is guidance on setting up the toolkit for use with Visual Studio here [https://www.asp.net/learn/ajax-videos/video-76.aspx](../../../videos/ajax-control-toolkit/how-do-i-get-started-with-the-aspnet-ajax-control-toolkit.md).</span></span>

<span data-ttu-id="68d6c-141">En el modo de diseño, arrastre los controles y controles de validación en el cuadro de herramientas y crear un formulario como la siguiente.</span><span class="sxs-lookup"><span data-stu-id="68d6c-141">In design mode, drag controls and validators from the toolbox and build a form like the one below.</span></span>

![](tailspin-spyworks-part-7/_static/image2.jpg)

<span data-ttu-id="68d6c-142">El marcado tendrá un aspecto algo parecido a esto.</span><span class="sxs-lookup"><span data-stu-id="68d6c-142">The markup will look something like this.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample8.aspx)]

<span data-ttu-id="68d6c-143">Ahora que podemos escribir las revisiones, permite mostrar las revisiones en la página del producto.</span><span class="sxs-lookup"><span data-stu-id="68d6c-143">Now that we can enter reviews, lets display those reviews on the product page.</span></span>

<span data-ttu-id="68d6c-144">Este marcado se agregue a la página ProductDetails.aspx.</span><span class="sxs-lookup"><span data-stu-id="68d6c-144">Add this markup to the ProductDetails.aspx page.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample9.aspx)]

<span data-ttu-id="68d6c-145">Ejecute ahora nuestra aplicación y navegar a un producto muestran la información del producto incluidas las revisiones de cliente.</span><span class="sxs-lookup"><span data-stu-id="68d6c-145">Running our application now and navigating to a product shows the product information including customer reviews.</span></span>

![](tailspin-spyworks-part-7/_static/image3.jpg)

## <a id="_Toc260221678"></a>  <span data-ttu-id="68d6c-146">Control de elementos más populares (creación de controles de usuario)</span><span class="sxs-lookup"><span data-stu-id="68d6c-146">Popular Items Control (Creating User Controls)</span></span>

<span data-ttu-id="68d6c-147">Con el fin de aumentar las ventas en el sitio web, se agregará un par de características a los productos de "venta sugeridos" populares o relacionados.</span><span class="sxs-lookup"><span data-stu-id="68d6c-147">In order to increase sales on your web site we will add a couple of features to "suggestive sell" popular or related products.</span></span>

<span data-ttu-id="68d6c-148">La primera de ellas será una lista de los productos más populares en nuestro catálogo de productos.</span><span class="sxs-lookup"><span data-stu-id="68d6c-148">The first of these features will be a list of the more popular product in our product catalog.</span></span>

<span data-ttu-id="68d6c-149">Crearemos un "Control de usuario" para mostrar los elementos en la página principal de nuestra aplicación más vendidos.</span><span class="sxs-lookup"><span data-stu-id="68d6c-149">We will create a "User Control" to display the top selling items on the home page of our application.</span></span> <span data-ttu-id="68d6c-150">Puesto que se trata de un control, nos podemos usar en cualquier página simplemente arrastrando y colocando el control en el Diseñador de Visual Studio en cualquier página que nos gusta.</span><span class="sxs-lookup"><span data-stu-id="68d6c-150">Since this will be a control, we can use it on any page by simply dragging and dropping the control in Visual Studio's designer onto any page that we like.</span></span>

<span data-ttu-id="68d6c-151">En el Explorador de soluciones de Visual Studio, haga doble clic en el nombre de la solución y cree un nuevo directorio denominado "Controles".</span><span class="sxs-lookup"><span data-stu-id="68d6c-151">In Visual Studio's solutions explorer, right-click on the solution name and create a new directory named "Controls".</span></span> <span data-ttu-id="68d6c-152">Aunque no es necesario hacerlo, nos ayudará a mantener nuestro proyecto organizada creando nuestro todos los controles de usuario en el directorio "Controles".</span><span class="sxs-lookup"><span data-stu-id="68d6c-152">While it is not necessary to do so, we will help keep our project organized by creating all our user controls in the "Controls" directory.</span></span>

<span data-ttu-id="68d6c-153">Haga doble clic en la carpeta controls y elija "Nuevo elemento":</span><span class="sxs-lookup"><span data-stu-id="68d6c-153">Right-click on the controls folder and choose "New Item" :</span></span>

![](tailspin-spyworks-part-7/_static/image4.jpg)

<span data-ttu-id="68d6c-154">Especifique un nombre para el control de "PopularItems".</span><span class="sxs-lookup"><span data-stu-id="68d6c-154">Specify a name for our control of "PopularItems".</span></span> <span data-ttu-id="68d6c-155">Tenga en cuenta que la extensión de archivo para los controles de usuario ascx .aspx no.</span><span class="sxs-lookup"><span data-stu-id="68d6c-155">Note that the file extension for user controls is .ascx not .aspx.</span></span>

<span data-ttu-id="68d6c-156">El control de usuario de los elementos más populares se definirá como sigue.</span><span class="sxs-lookup"><span data-stu-id="68d6c-156">Our Popular Items User control will be defined as follows.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample10.aspx)]

<span data-ttu-id="68d6c-157">Vamos a usar un método que no hemos usado aún en esta aplicación.</span><span class="sxs-lookup"><span data-stu-id="68d6c-157">Here we're using a method we have not used yet in this application.</span></span> <span data-ttu-id="68d6c-158">Estamos usando el control repeater y en lugar de usar un control de origen de datos enlazamos el Control Repeater a los resultados de una consulta LINQ to Entities.</span><span class="sxs-lookup"><span data-stu-id="68d6c-158">We're using the repeater control and instead of using a data source control we're binding the Repeater Control to the results of a LINQ to Entities query.</span></span>

<span data-ttu-id="68d6c-159">En el código subyacente de nuestro control hacerlo como sigue.</span><span class="sxs-lookup"><span data-stu-id="68d6c-159">In the code behind of our control we do that as follows.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample11.cs)]

<span data-ttu-id="68d6c-160">Tenga en cuenta también esta línea importante en la parte superior del marcado de nuestro control.</span><span class="sxs-lookup"><span data-stu-id="68d6c-160">Note also this important line at the top of our control's markup.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample12.aspx)]

<span data-ttu-id="68d6c-161">Puesto que no cambiará los elementos más populares minuto a minuto, podemos agregar una directiva dolor para mejorar el rendimiento de nuestra aplicación.</span><span class="sxs-lookup"><span data-stu-id="68d6c-161">Since the most popular items won't be changing on a minute to minute basis we can add a aching directive to improve the performance of our application.</span></span> <span data-ttu-id="68d6c-162">Esta directiva hará que el código de los controles que solo se ejecutará cuando expira la salida almacenada en caché del control.</span><span class="sxs-lookup"><span data-stu-id="68d6c-162">This directive will cause the controls code to only be executed when the cached output of the control expires.</span></span> <span data-ttu-id="68d6c-163">En caso contrario, se usará la versión en caché de salida del control.</span><span class="sxs-lookup"><span data-stu-id="68d6c-163">Otherwise, the cached version of the control's output will be used.</span></span>

<span data-ttu-id="68d6c-164">Ahora sólo tenemos que hacer es incluir el control de nuevo en nuestra página Default.aspc.</span><span class="sxs-lookup"><span data-stu-id="68d6c-164">Now all we have to do is include our new control in our Default.aspc page.</span></span>

<span data-ttu-id="68d6c-165">Utilice arrastrar y colocar para colocar una instancia del control en la columna abierta el formulario predeterminado.</span><span class="sxs-lookup"><span data-stu-id="68d6c-165">Use drag and drop to place an instance of the control in the open column of our Default form.</span></span>

![](tailspin-spyworks-part-7/_static/image5.jpg)

<span data-ttu-id="68d6c-166">Ahora cuando ejecutamos nuestra aplicación de la página principal muestran los elementos más populares.</span><span class="sxs-lookup"><span data-stu-id="68d6c-166">Now when we run our application the home page displays the most popular items.</span></span>

![](tailspin-spyworks-part-7/_static/image6.jpg)

## <a id="_Toc260221679"></a>  <span data-ttu-id="68d6c-167">"También adquirido" controlar (controles de usuario con parámetros)</span><span class="sxs-lookup"><span data-stu-id="68d6c-167">"Also Purchased" Control (User Controls with Parameters)</span></span>

<span data-ttu-id="68d6c-168">El segundo Control de usuario que vamos a crear tendrá sugerido de venta al siguiente nivel mediante la adición de especificidad de contexto.</span><span class="sxs-lookup"><span data-stu-id="68d6c-168">The second User Control that we'll create will take suggestive selling to the next level by adding context specificity.</span></span>

<span data-ttu-id="68d6c-169">La lógica para calcular los elementos superiores "También adquirido" no es trivial.</span><span class="sxs-lookup"><span data-stu-id="68d6c-169">The logic to calculate the top "Also Purchased" items is non-trivial.</span></span>

<span data-ttu-id="68d6c-170">Nuestro control "También adquirido" seleccionar los registros de OrderDetails (comprados anteriormente) para el ProductID seleccionado actualmente y captar los OrderID para cada pedido único que se encuentra.</span><span class="sxs-lookup"><span data-stu-id="68d6c-170">Our "Also Purchased" control will select the OrderDetails records (previously purchased) for the currently selected ProductID and grab the OrderIDs for each unique order that is found.</span></span>

<span data-ttu-id="68d6c-171">A continuación, vamos a seleccionar a los productos de todos los pedidos y suma las cantidades adquiridas.</span><span class="sxs-lookup"><span data-stu-id="68d6c-171">Then we will select al the products from all those Orders and sum the quantities purchased.</span></span> <span data-ttu-id="68d6c-172">Comenzaremos ordenar los productos que suman una cantidad y mostrar los elementos de los cinco principales.</span><span class="sxs-lookup"><span data-stu-id="68d6c-172">We'll sort the products by that quantity sum and display the top five items.</span></span>

<span data-ttu-id="68d6c-173">Dada la complejidad de esta lógica, se implementará este algoritmo como un procedimiento almacenado.</span><span class="sxs-lookup"><span data-stu-id="68d6c-173">Given the complexity of this logic, we will implement this algorithm as a stored procedure.</span></span>

<span data-ttu-id="68d6c-174">La instrucción T-SQL para el procedimiento almacenado es como sigue.</span><span class="sxs-lookup"><span data-stu-id="68d6c-174">The T-SQL for the stored procedure is as follows.</span></span>

[!code-sql[Main](tailspin-spyworks-part-7/samples/sample13.sql)]

<span data-ttu-id="68d6c-175">Tenga en cuenta que este procedimiento almacenado (SelectPurchasedWithProducts) existía en la base de datos cuando se incluyen en nuestra aplicación y cuando se genera el Entity Data Model se especifica que, además de las tablas y vistas que debíamos, Entity Data Model debe incluir este procedimiento almacenado.</span><span class="sxs-lookup"><span data-stu-id="68d6c-175">Note that this stored procedure (SelectPurchasedWithProducts) existed in the database when we included it in our application and when we generated the Entity Data Model we specified that, in addition to the Tables and Views that we needed, the Entity Data Model should include this stored procedure.</span></span>

<span data-ttu-id="68d6c-176">Para tener acceso al procedimiento almacenado de Entity Data Model se debe importar la función.</span><span class="sxs-lookup"><span data-stu-id="68d6c-176">To access the stored procedure from the Entity Data Model we need to import the function.</span></span>

<span data-ttu-id="68d6c-177">Haga doble clic en el Entity Data Model en el Explorador de soluciones para abrirlo en el diseñador y abra el Explorador de modelos, a continuación, haga clic en el diseñador y seleccione "Agregar importación de función".</span><span class="sxs-lookup"><span data-stu-id="68d6c-177">Double Click on the Entity Data Model in the Solutions Explorer to open it in the designer and open the Model Browser, then right-click in the designer and select "Add Function Import".</span></span>

![](tailspin-spyworks-part-7/_static/image1.png)

<span data-ttu-id="68d6c-178">Si lo hace, se abrirá este cuadro de diálogo.</span><span class="sxs-lookup"><span data-stu-id="68d6c-178">Doing so will open this dialog.</span></span>

![](tailspin-spyworks-part-7/_static/image2.png)

<span data-ttu-id="68d6c-179">Rellene los campos como observar más arriba, seleccionando la "SelectPurchasedWithProducts" y use el nombre del procedimiento para el nombre de la función importada.</span><span class="sxs-lookup"><span data-stu-id="68d6c-179">Fill out the fields as you see above, selecting the "SelectPurchasedWithProducts" and use the procedure name for the name of our imported function.</span></span>

<span data-ttu-id="68d6c-180">Haga clic en "Aceptar".</span><span class="sxs-lookup"><span data-stu-id="68d6c-180">Click "Ok".</span></span>

<span data-ttu-id="68d6c-181">Hecho esto que podemos simplemente programaremos con el procedimiento almacenado como se podría cualquier otro elemento en el modelo.</span><span class="sxs-lookup"><span data-stu-id="68d6c-181">Having done this we can simply program against the stored procedure as we might any other item in the model.</span></span>

<span data-ttu-id="68d6c-182">Por lo tanto, en la carpeta "Controles" crear un nuevo control de usuario denominado AlsoPurchased.ascx.</span><span class="sxs-lookup"><span data-stu-id="68d6c-182">So, in our "Controls" folder create a new user control named AlsoPurchased.ascx.</span></span>

<span data-ttu-id="68d6c-183">El marcado para este control le parecerán conocido para el control PopularItems.</span><span class="sxs-lookup"><span data-stu-id="68d6c-183">The markup for this control will look very familiar to the PopularItems control.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample14.aspx)]

<span data-ttu-id="68d6c-184">La diferencia notable es que son no almacena en caché la salida desde que se va a representar el elemento variarán por producto.</span><span class="sxs-lookup"><span data-stu-id="68d6c-184">The notable difference is that are not caching the output since the item's to be rendered will differ by product.</span></span>

<span data-ttu-id="68d6c-185">El ProductId será "property" para el control.</span><span class="sxs-lookup"><span data-stu-id="68d6c-185">The ProductId will be a "property" to the control.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample15.cs)]

<span data-ttu-id="68d6c-186">En el controlador de evento PreRender del control se eed hacer tres cosas.</span><span class="sxs-lookup"><span data-stu-id="68d6c-186">In the control's PreRender event handler we eed to do three things.</span></span>

1. <span data-ttu-id="68d6c-187">Asegúrese de que se ha establecido el ProductID.</span><span class="sxs-lookup"><span data-stu-id="68d6c-187">Make sure the ProductID is set.</span></span>
2. <span data-ttu-id="68d6c-188">Consulte si hay productos que se han adquirido con la actual.</span><span class="sxs-lookup"><span data-stu-id="68d6c-188">See if there are any products that have been purchased with the current one.</span></span>
3. <span data-ttu-id="68d6c-189">Algunos elementos, como se determina en #2 de salida.</span><span class="sxs-lookup"><span data-stu-id="68d6c-189">Output some items as determined in #2.</span></span>

<span data-ttu-id="68d6c-190">Tenga en cuenta lo fácil que es llamar al procedimiento almacenado mediante el modelo.</span><span class="sxs-lookup"><span data-stu-id="68d6c-190">Note how easy it is to call the stored procedure through the model.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample16.cs)]

<span data-ttu-id="68d6c-191">Después de determinar que no existe "también adquieren" simplemente podemos enlazar el control repeater a los resultados devueltos por la consulta.</span><span class="sxs-lookup"><span data-stu-id="68d6c-191">After determining that there ARE "also purchased" we can simply bind the repeater to the results returned by the query.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample17.cs)]

<span data-ttu-id="68d6c-192">Si no hubiera ningún elemento "también adquirido" simplemente mostraremos otros elementos más populares de nuestro catálogo.</span><span class="sxs-lookup"><span data-stu-id="68d6c-192">If there were not any "also purchased" items we'll simply display other popular items from our catalog.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample18.cs)]

<span data-ttu-id="68d6c-193">Para ver los elementos "También comprar", abra la página ProductDetails.aspx y arrastre el control AlsoPurchased desde el Explorador de soluciones para que aparezca en esta posición en el marcado.</span><span class="sxs-lookup"><span data-stu-id="68d6c-193">To view the "Also Purchased" items, open the ProductDetails.aspx page and drag the AlsoPurchased control from the Solutions Explorer so that it appears in this position in the markup.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample19.aspx)]

<span data-ttu-id="68d6c-194">Si lo hace, creará una referencia al control en la parte superior de la página ProductDetails.</span><span class="sxs-lookup"><span data-stu-id="68d6c-194">Doing so will create a reference to the control at the top of the ProductDetails page.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample20.aspx)]

<span data-ttu-id="68d6c-195">Puesto que el control de usuario AlsoPurchased requiere un número de ProductId se establecerá la propiedad ProductID de nuestro control mediante el uso de una instrucción Eval en el elemento de modelo de datos actual de la página.</span><span class="sxs-lookup"><span data-stu-id="68d6c-195">Since the AlsoPurchased user control requires a ProductId number we will set the ProductID property of our control by using an Eval statement against the current data model item of the page.</span></span>

![](tailspin-spyworks-part-7/_static/image3.png)

<span data-ttu-id="68d6c-196">Cuando nos compilar y ejecutar ahora y vaya a un producto vemos los elementos "Adquirido también".</span><span class="sxs-lookup"><span data-stu-id="68d6c-196">When we build and run now and browse to a product we see the "Also Purchased" items.</span></span>

![](tailspin-spyworks-part-7/_static/image7.jpg)

> [!div class="step-by-step"]
> <span data-ttu-id="68d6c-197">[Anterior](tailspin-spyworks-part-6.md)
> [Siguiente](tailspin-spyworks-part-8.md)</span><span class="sxs-lookup"><span data-stu-id="68d6c-197">[Previous](tailspin-spyworks-part-6.md)
[Next](tailspin-spyworks-part-8.md)</span></span>
