---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
title: 'Parte 5: Lógica de negocios | Microsoft Docs'
author: JoeStagner
description: Esta serie de tutoriales detalla todos los pasos que se tarda en compilar la aplicación de ejemplo Tailspin Spyworks. Parte 5 agrega alguna lógica de negocios.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: eaef475a-ca91-47ea-a4a7-d074005ed80c
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
msc.type: authoredcontent
ms.openlocfilehash: 719d56e0764e2f66b8813c9487119bbc700d738c
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37377991"
---
<a name="part-5-business-logic"></a><span data-ttu-id="a6298-104">Parte 5: Lógica de negocios</span><span class="sxs-lookup"><span data-stu-id="a6298-104">Part 5: Business Logic</span></span>
====================
<span data-ttu-id="a6298-105">por [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="a6298-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="a6298-106">Tailspin Spyworks demuestra cómo extraordinariamente simple es crear aplicaciones eficaces y escalables para la plataforma. NET.</span><span class="sxs-lookup"><span data-stu-id="a6298-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="a6298-107">Resalta cómo usar las características nuevas en ASP.NET 4 para crear una tienda en línea, incluida la compra, la desprotección y la administración.</span><span class="sxs-lookup"><span data-stu-id="a6298-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="a6298-108">Esta serie de tutoriales detalla todos los pasos que se tarda en compilar la aplicación de ejemplo Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="a6298-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="a6298-109">Parte 5 agrega alguna lógica de negocios.</span><span class="sxs-lookup"><span data-stu-id="a6298-109">Part 5 adds some business logic.</span></span>


## <a id="_Toc260221671"></a>  <span data-ttu-id="a6298-110">Adición de lógica de negocios</span><span class="sxs-lookup"><span data-stu-id="a6298-110">Adding Some Business Logic</span></span>

<span data-ttu-id="a6298-111">Queremos que nuestra experiencia de compra esté disponible cada vez que alguien visite nuestro sitio web.</span><span class="sxs-lookup"><span data-stu-id="a6298-111">We want our shopping experience to be available whenever someone visits our web site.</span></span> <span data-ttu-id="a6298-112">Los visitantes podrán examinar y agregar elementos al carro de la compra incluso si no están registrados ni ha iniciado sesión.</span><span class="sxs-lookup"><span data-stu-id="a6298-112">Visitors will be able to browse and add items to the shopping cart even if they are not registered or logged in.</span></span> <span data-ttu-id="a6298-113">Cuando estén listas desproteger se les dará la opción de autenticar y si no lo son aún los miembros podrán crear una cuenta.</span><span class="sxs-lookup"><span data-stu-id="a6298-113">When they are ready to check out they will be given the option to authenticate and if they are not yet members they will be able to create an account.</span></span>

<span data-ttu-id="a6298-114">Esto significa que debemos implementar la lógica para convertir el carro de la compra de un estado anónimo a un estado de "Usuario registrado".</span><span class="sxs-lookup"><span data-stu-id="a6298-114">This means that we will need to implement the logic to convert the shopping cart from an anonymous state to a "Registered User" state.</span></span>

<span data-ttu-id="a6298-115">Vamos a crear un directorio denominado "Clases", a continuación, haga doble clic en la carpeta y cree un nuevo archivo de "Class" denominado MyShoppingCart.cs</span><span class="sxs-lookup"><span data-stu-id="a6298-115">Let's create a directory named "Classes" then Right-Click on the folder and create a new "Class" file named MyShoppingCart.cs</span></span>

![](tailspin-spyworks-part-5/_static/image1.jpg)

![](tailspin-spyworks-part-5/_static/image1.png)

<span data-ttu-id="a6298-116">Como se mencionó anteriormente ampliaremos la clase que implementa la página MyShoppingCart.aspx y se realizará este paso mediante. Construcción de la red eficaz "clase parcial".</span><span class="sxs-lookup"><span data-stu-id="a6298-116">As previously mentioned we will be extending the class that implements the MyShoppingCart.aspx page and we will do this using .NET's powerful "Partial Class" construct.</span></span>

<span data-ttu-id="a6298-117">La llamada para nuestro archivo MyShoppingCart.aspx.cf generada tiene este aspecto.</span><span class="sxs-lookup"><span data-stu-id="a6298-117">The generated call for our MyShoppingCart.aspx.cf file looks like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample1.cs)]

<span data-ttu-id="a6298-118">Tenga en cuenta el uso de la palabra clave "parcial".</span><span class="sxs-lookup"><span data-stu-id="a6298-118">Note the use of the "partial" keyword.</span></span>

<span data-ttu-id="a6298-119">El archivo de clase que se acaba de generar tiene este aspecto.</span><span class="sxs-lookup"><span data-stu-id="a6298-119">The class file that we just generated looks like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample2.cs)]

<span data-ttu-id="a6298-120">Combinaremos nuestras implementaciones mediante la adición de la palabra clave parcial a este archivo también.</span><span class="sxs-lookup"><span data-stu-id="a6298-120">We will merge our implementations by adding the partial keyword to this file as well.</span></span>

<span data-ttu-id="a6298-121">Nuestro nuevo archivo de clase ahora este aspecto.</span><span class="sxs-lookup"><span data-stu-id="a6298-121">Our new class file now looks like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample3.cs)]

<span data-ttu-id="a6298-122">El primer método que agregamos a nuestra clase es el método "AddItem".</span><span class="sxs-lookup"><span data-stu-id="a6298-122">The first method that we will add to our class is the "AddItem" method.</span></span> <span data-ttu-id="a6298-123">Este es el método que se llamará en última instancia, cuando el usuario hace clic en los vínculos de "Agregar a imágenes" en las páginas de lista de productos y los detalles del producto.</span><span class="sxs-lookup"><span data-stu-id="a6298-123">This is the method that will ultimately be called when the user clicks on the "Add to Art" links on the Product List and Product Details pages.</span></span>

<span data-ttu-id="a6298-124">Agregue lo siguiente para el uso de instrucciones en la parte superior de la página.</span><span class="sxs-lookup"><span data-stu-id="a6298-124">Append the following to the using statements at the top of the page.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample4.cs)]

<span data-ttu-id="a6298-125">Y agregue este método a la clase MyShoppingCart.</span><span class="sxs-lookup"><span data-stu-id="a6298-125">And add this method to the MyShoppingCart class.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample5.cs)]

<span data-ttu-id="a6298-126">Estamos usando LINQ to Entities para ver si el elemento ya está en el carro de compra.</span><span class="sxs-lookup"><span data-stu-id="a6298-126">We are using LINQ to Entities to see if the item is already in the cart.</span></span> <span data-ttu-id="a6298-127">Si por lo tanto, se actualiza la cantidad del pedido del elemento, en caso contrario, se creará una nueva entrada para el elemento seleccionado</span><span class="sxs-lookup"><span data-stu-id="a6298-127">If so, we update the order quantity of the item, otherwise we create a new entry for the selected item</span></span>

<span data-ttu-id="a6298-128">Para poder llamar a este método se implementará una página AddToCart.aspx que no solo este método de clase, pero, a continuación, muestra un carro = de la compra después de que se ha agregado el elemento actual.</span><span class="sxs-lookup"><span data-stu-id="a6298-128">In order to call this method we will implement an AddToCart.aspx page that not only class this method but then displayed the current shopping a=cart after the item has been added.</span></span>

<span data-ttu-id="a6298-129">Haga doble clic en el nombre de la solución en el Explorador de soluciones y agregue y nueva página denominada AddToCart.aspx como se ha hecho anteriormente.</span><span class="sxs-lookup"><span data-stu-id="a6298-129">Right-Click on the solution name in the solution explorer and add and new page named AddToCart.aspx as we have done previously.</span></span>

<span data-ttu-id="a6298-130">Aunque podríamos usamos esta página para mostrar los resultados provisionales como baja problemas stock, etcetera, en nuestra implementación, la página realmente no representan, pero en su lugar llamará a la lógica de "Agregar" y redirigir.</span><span class="sxs-lookup"><span data-stu-id="a6298-130">While we could use this page to display interim results like low stock issues, etc, in our implementation, the page will not actually render, but rather call the "Add" logic and redirect.</span></span>

<span data-ttu-id="a6298-131">Para lograr esto, vamos a agregar el código siguiente a la página\_evento Load.</span><span class="sxs-lookup"><span data-stu-id="a6298-131">To accomplish this we'll add the following code to the Page\_Load event.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample6.cs)]

<span data-ttu-id="a6298-132">Tenga en cuenta que estamos recuperando el producto para agregar al carro de la compra de un parámetro de cadena de consulta y una llamada al método AddItem de nuestra clase.</span><span class="sxs-lookup"><span data-stu-id="a6298-132">Note that we are retrieving the product to add to the shopping cart from a QueryString parameter and calling the AddItem method of our class.</span></span>

<span data-ttu-id="a6298-133">Suponiendo que no hay errores se encuentran el control pasa a la página SHoppingCart.aspx que totalmente se implementará a continuación.</span><span class="sxs-lookup"><span data-stu-id="a6298-133">Assuming no errors are encountered control is passed to the SHoppingCart.aspx page which we will fully implement next.</span></span> <span data-ttu-id="a6298-134">Si debe haber un error se produce una excepción.</span><span class="sxs-lookup"><span data-stu-id="a6298-134">If there should be an error we throw an exception.</span></span>

<span data-ttu-id="a6298-135">Actualmente, no hemos implementado todavía un controlador de errores global para esta excepción iría no controlada por nuestra aplicación, pero se solucionará esto en breve.</span><span class="sxs-lookup"><span data-stu-id="a6298-135">Currently we have not yet implemented a global error handler so this exception would go unhandled by our application but we will remedy this shortly.</span></span>

<span data-ttu-id="a6298-136">Tenga en cuenta también el uso de la instrucción Debug.Fail() (disponible a través de `using System.Diagnostics;)`</span><span class="sxs-lookup"><span data-stu-id="a6298-136">Note also the use of the statement Debug.Fail() (available via `using System.Diagnostics;)`</span></span>

<span data-ttu-id="a6298-137">Es la aplicación se ejecuta en el depurador, este método mostrará un cuadro de diálogo detallada con información sobre el estado de las aplicaciones junto con el mensaje de error que se especifique.</span><span class="sxs-lookup"><span data-stu-id="a6298-137">Is the application is running inside the debugger, this method will display a detailed dialog with information about the applications state along with the error message that we specify.</span></span>

<span data-ttu-id="a6298-138">Cuando se ejecuta en producción el Debug.Fail() se omite la instrucción.</span><span class="sxs-lookup"><span data-stu-id="a6298-138">When running in production the Debug.Fail() statement is ignored.</span></span>

<span data-ttu-id="a6298-139">Puede observar en el código anterior de una llamada a un método en nuestros nombres de clase de carro de la compra "GetShoppingCartId".</span><span class="sxs-lookup"><span data-stu-id="a6298-139">You will note in the code above a call to a method in our shopping cart class names "GetShoppingCartId".</span></span>

<span data-ttu-id="a6298-140">Agregue el código para implementar el método como sigue.</span><span class="sxs-lookup"><span data-stu-id="a6298-140">Add the code to implement the method as follows.</span></span>

<span data-ttu-id="a6298-141">Tenga en cuenta que también hemos agregado los botones de actualización y la desprotección y una etiqueta donde podemos mostrar el carro de compra "total".</span><span class="sxs-lookup"><span data-stu-id="a6298-141">Note that we've also added update and checkout buttons and a label where we can display the cart "total".</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample7.cs)]

<span data-ttu-id="a6298-142">Ahora podemos agregar elementos a nuestro carro de compra, pero no hemos implementado la lógica para mostrar el carro de compra una vez se ha agregado un producto.</span><span class="sxs-lookup"><span data-stu-id="a6298-142">We can now add items to our shopping cart but we have not implemented the logic to display the cart after a product has been added.</span></span>

<span data-ttu-id="a6298-143">Por lo tanto, en la página MyShoppingCart.aspx agregaremos un control EntityDataSource y un control GridVire como sigue.</span><span class="sxs-lookup"><span data-stu-id="a6298-143">So, in the MyShoppingCart.aspx page we'll add an EntityDataSource control and a GridVire control as follows.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample8.aspx)]

<span data-ttu-id="a6298-144">Llame el formulario en el diseñador para que pueda hacer doble clic en el botón actualizar el carro y generar el controlador de eventos click que se especifica en la declaración en el marcado.</span><span class="sxs-lookup"><span data-stu-id="a6298-144">Call up the form in the designer so that you can double click on the Update Cart button and generate the click event handler that is specified in the declaration in the markup.</span></span>

<span data-ttu-id="a6298-145">Implementaremos los detalles más adelante, pero hacerlo Permítanos compilará y ejecutará nuestra aplicación sin errores.</span><span class="sxs-lookup"><span data-stu-id="a6298-145">We'll implement the details later but doing this will let us build and run our application without errors.</span></span>

<span data-ttu-id="a6298-146">Cuando se ejecuta la aplicación y agregar un elemento al carro de la compra verá lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="a6298-146">When you run the application and add an item to the shopping cart you will see this.</span></span>

![](tailspin-spyworks-part-5/_static/image2.jpg)

<span data-ttu-id="a6298-147">Tenga en cuenta que se han desviado de la presentación de cuadrícula "default" mediante la implementación de tres columnas personalizadas.</span><span class="sxs-lookup"><span data-stu-id="a6298-147">Note that we have deviated from the "default" grid display by implementing three custom columns.</span></span>

<span data-ttu-id="a6298-148">El primero es un Editable, el campo "Enlace" para la cantidad de:</span><span class="sxs-lookup"><span data-stu-id="a6298-148">The first is an Editable, "Bound" field for the Quantity:</span></span>

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample9.aspx)]

<span data-ttu-id="a6298-149">La siguiente es una columna "calculada" que muestra el elemento de línea total (el elemento de coste multiplicado por la cantidad se ordenen):</span><span class="sxs-lookup"><span data-stu-id="a6298-149">The next is a "calculated" column that displays the line item total (the item cost times the quantity to be ordered):</span></span>

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample10.aspx)]

<span data-ttu-id="a6298-150">Por último, tenemos una columna personalizada que contiene un control de casilla de verificación que el usuario va a usar para indicar que el elemento debe quitarse del plan de compra.</span><span class="sxs-lookup"><span data-stu-id="a6298-150">Lastly we have a custom column that contains a CheckBox control that the user will use to indicate that the item should be removed from the shopping chart.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample11.aspx)]

![](tailspin-spyworks-part-5/_static/image3.jpg)

<span data-ttu-id="a6298-151">Como puede ver, el orden Total de líneas está vacío, así que vamos a agregar lógica para calcular el Total del pedido.</span><span class="sxs-lookup"><span data-stu-id="a6298-151">As you can see, the Order Total line is empty so let's add some logic to calculate the Order Total.</span></span>

<span data-ttu-id="a6298-152">En primer lugar implementaremos un método "GetTotal" a nuestra clase MyShoppingCart.</span><span class="sxs-lookup"><span data-stu-id="a6298-152">We'll first implement a "GetTotal" method to our MyShoppingCart Class.</span></span>

<span data-ttu-id="a6298-153">En el archivo MyShoppingCart.cs agregue el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="a6298-153">In the MyShoppingCart.cs file add the following code.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample12.cs)]

<span data-ttu-id="a6298-154">A continuación, en la página\_controlador de eventos de carga puede llamaremos a nuestro método GetTotal.</span><span class="sxs-lookup"><span data-stu-id="a6298-154">Then in the Page\_Load event handler we'll can call our GetTotal method.</span></span> <span data-ttu-id="a6298-155">Al mismo tiempo, vamos a agregar una prueba para comprobar si el carro de la compra está vacía y ajuste la presentación en consecuencia, si se trata.</span><span class="sxs-lookup"><span data-stu-id="a6298-155">At the same time we'll add a test to see if the shopping cart is empty and adjust the display accordingly if it is.</span></span>

<span data-ttu-id="a6298-156">Ahora si el carro de la compra está vacía, obtener esto:</span><span class="sxs-lookup"><span data-stu-id="a6298-156">Now if the shopping cart is empty we get this:</span></span>

![](tailspin-spyworks-part-5/_static/image4.jpg)

<span data-ttu-id="a6298-157">Y si no, vemos que nuestro total.</span><span class="sxs-lookup"><span data-stu-id="a6298-157">And if not, we see our total.</span></span>

![](tailspin-spyworks-part-5/_static/image5.jpg)

<span data-ttu-id="a6298-158">Sin embargo, esta página no está completa.</span><span class="sxs-lookup"><span data-stu-id="a6298-158">However, this page is not yet complete.</span></span>

<span data-ttu-id="a6298-159">Necesitamos una lógica adicional para volver a calcular el carro de la compra mediante la eliminación de los elementos marcados para su eliminación y determinando los nuevos valores de cantidad como algunos es posible que se han cambiado en la cuadrícula por el usuario.</span><span class="sxs-lookup"><span data-stu-id="a6298-159">We will need additional logic to recalculate the shopping cart by removing items marked for removal and by determining new quantity values as some may have been changed in the grid by the user.</span></span>

<span data-ttu-id="a6298-160">Permite agregar un método "RemoveItem" a nuestra clase de carro de la en MyShoppingCart.cs para controlar el caso cuando un usuario marca un elemento para su eliminación.</span><span class="sxs-lookup"><span data-stu-id="a6298-160">Lets add a "RemoveItem" method to our shopping cart class in MyShoppingCart.cs to handle the case when a user marks an item for removal.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample13.cs)]

<span data-ttu-id="a6298-161">Ahora vamos a ad un método para controlar el caso cuando un usuario simplemente cambia la calidad se ordenen en GridView.</span><span class="sxs-lookup"><span data-stu-id="a6298-161">Now let's ad a method to handle the circumstance when a user simply changes the quality to be ordered in the GridView.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample14.cs)]

<span data-ttu-id="a6298-162">Podemos implementar la lógica que realmente actualiza el carro de la compra en la base de datos con las características básicas de eliminación y actualización en su lugar.</span><span class="sxs-lookup"><span data-stu-id="a6298-162">With the basic Remove and Update features in place we can implement the logic that actually updates the shopping cart in the database.</span></span> <span data-ttu-id="a6298-163">(En MyShoppingCart.cs)</span><span class="sxs-lookup"><span data-stu-id="a6298-163">(In MyShoppingCart.cs)</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample15.cs)]

<span data-ttu-id="a6298-164">Observará que este método espera dos parámetros.</span><span class="sxs-lookup"><span data-stu-id="a6298-164">You'll note that this method expects two parameters.</span></span> <span data-ttu-id="a6298-165">Uno es el Id. de carro y el otro es una matriz de objetos de tipo definido por el usuario.</span><span class="sxs-lookup"><span data-stu-id="a6298-165">One is the shopping cart Id and the other is an array of objects of user defined type.</span></span>

<span data-ttu-id="a6298-166">Con el fin de minimizar la dependencia de nuestra lógica sobre características específicas de interfaz de usuario, hemos definido una estructura de datos que podemos usar para pasar los elementos del carro de la compra a nuestro código sin nuestro método necesite acceder directamente el control GridView.</span><span class="sxs-lookup"><span data-stu-id="a6298-166">So as to minimize the dependency of our logic on user interface specifics, we've defined a data structure that we can use to pass the shopping cart items to our code without our method needing to directly access the GridView control.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample16.cs)]

<span data-ttu-id="a6298-167">En nuestro archivo MyShoppingCart.aspx.cs podemos usar esta estructura en el controlador de evento Click de botón de actualización como se indica a continuación.</span><span class="sxs-lookup"><span data-stu-id="a6298-167">In our MyShoppingCart.aspx.cs file we can use this structure in our Update Button Click Event handler as follows.</span></span> <span data-ttu-id="a6298-168">Tenga en cuenta que además de actualizar el carro de compra se actualizará también el total del carro de compra.</span><span class="sxs-lookup"><span data-stu-id="a6298-168">Note that in addition to updating the cart we will update the cart total as well.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample17.cs)]

<span data-ttu-id="a6298-169">Tenga en cuenta con un interés particular esta línea de código:</span><span class="sxs-lookup"><span data-stu-id="a6298-169">Note with particular interest this line of code:</span></span>

[!code-javascript[Main](tailspin-spyworks-part-5/samples/sample18.js)]

<span data-ttu-id="a6298-170">GetValues() es una función auxiliar especial que se implementará en MyShoppingCart.aspx.cs como sigue.</span><span class="sxs-lookup"><span data-stu-id="a6298-170">GetValues() is a special helper function that we will implement in MyShoppingCart.aspx.cs as follows.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample19.cs)]

<span data-ttu-id="a6298-171">Esto proporciona una manera limpia para tener acceso a los valores de los elementos enlazados de nuestro control GridView.</span><span class="sxs-lookup"><span data-stu-id="a6298-171">This provides a clean way to access the values of the bound elements in our GridView control.</span></span> <span data-ttu-id="a6298-172">Puesto que no está enlazado el Control de casilla de verificación "Quitar el elemento" se tendrá acceso a él a través del método FindControl().</span><span class="sxs-lookup"><span data-stu-id="a6298-172">Since our "Remove Item" CheckBox Control is not bound we'll access it via the FindControl() method.</span></span>

<span data-ttu-id="a6298-173">En esta fase de desarrollo del proyecto estamos preparados implementar el proceso de pago.</span><span class="sxs-lookup"><span data-stu-id="a6298-173">At this stage in your project's development we are getting ready to implement the checkout process.</span></span>

<span data-ttu-id="a6298-174">Antes de que al hacerlo, vamos a usar Visual Studio para generar la base de datos de pertenencia y agregar un usuario en el repositorio de pertenencia.</span><span class="sxs-lookup"><span data-stu-id="a6298-174">Before doing so let's use Visual Studio to generate the membership database and add a user to the membership repository.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a6298-175">[Anterior](tailspin-spyworks-part-4.md)
> [Siguiente](tailspin-spyworks-part-6.md)</span><span class="sxs-lookup"><span data-stu-id="a6298-175">[Previous](tailspin-spyworks-part-4.md)
[Next](tailspin-spyworks-part-6.md)</span></span>
