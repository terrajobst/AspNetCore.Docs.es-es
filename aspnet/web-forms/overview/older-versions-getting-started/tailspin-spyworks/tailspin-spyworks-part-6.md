---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
title: 'Parte 6: Pertenencia ASP.NET | Microsoft Docs'
author: JoeStagner
description: Esta serie de tutoriales detalla todos los pasos que se tarda en compilar la aplicación de ejemplo Tailspin Spyworks. Parte 6 agrega la pertenencia a ASP.NET.
ms.author: aspnetcontent
ms.date: 07/21/2010
ms.assetid: f70a310c-9557-4743-82cb-655265676d39
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
msc.type: authoredcontent
ms.openlocfilehash: 303c1edf548db1da9ef61d94bdd8157d6afbb2e4
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37804425"
---
<a name="part-6-aspnet-membership"></a><span data-ttu-id="036a4-104">Parte 6: Pertenencia ASP.NET</span><span class="sxs-lookup"><span data-stu-id="036a4-104">Part 6: ASP.NET Membership</span></span>
====================
<span data-ttu-id="036a4-105">por [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="036a4-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="036a4-106">Tailspin Spyworks demuestra cómo extraordinariamente simple es crear aplicaciones eficaces y escalables para la plataforma. NET.</span><span class="sxs-lookup"><span data-stu-id="036a4-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="036a4-107">Resalta cómo usar las características nuevas en ASP.NET 4 para crear una tienda en línea, incluida la compra, la desprotección y la administración.</span><span class="sxs-lookup"><span data-stu-id="036a4-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="036a4-108">Esta serie de tutoriales detalla todos los pasos que se tarda en compilar la aplicación de ejemplo Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="036a4-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="036a4-109">Parte 6 agrega la pertenencia a ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="036a4-109">Part 6 adds ASP.NET Membership.</span></span>


## <a id="_Toc260221672"></a>  <span data-ttu-id="036a4-110">Trabajar con la pertenencia a ASP.NET</span><span class="sxs-lookup"><span data-stu-id="036a4-110">Working with ASP.NET Membership</span></span>

![](tailspin-spyworks-part-6/_static/image1.png)

<span data-ttu-id="036a4-111">Haga clic en seguridad</span><span class="sxs-lookup"><span data-stu-id="036a4-111">Click Security</span></span>

![](tailspin-spyworks-part-6/_static/image1.jpg)

<span data-ttu-id="036a4-112">Asegúrese de que estamos usando autenticación de formularios.</span><span class="sxs-lookup"><span data-stu-id="036a4-112">Make sure that we are using forms authentication.</span></span>

![](tailspin-spyworks-part-6/_static/image2.jpg)

<span data-ttu-id="036a4-113">Use el vínculo "Crear usuario" para crear un par de usuarios.</span><span class="sxs-lookup"><span data-stu-id="036a4-113">Use the "Create User" link to create a couple of users.</span></span>

![](tailspin-spyworks-part-6/_static/image3.jpg)

<span data-ttu-id="036a4-114">Cuando haya terminado, consulte la ventana Explorador de soluciones y actualice la vista.</span><span class="sxs-lookup"><span data-stu-id="036a4-114">When done, refer to the Solution Explorer window and refresh the view.</span></span>

![](tailspin-spyworks-part-6/_static/image2.png)

<span data-ttu-id="036a4-115">Tenga en cuenta que el ASPNETDB. MDF bien se ha creado.</span><span class="sxs-lookup"><span data-stu-id="036a4-115">Note that the ASPNETDB.MDF fine has been created.</span></span> <span data-ttu-id="036a4-116">Este archivo contiene las tablas para admitir los servicios de ASP.NET core, como la pertenencia.</span><span class="sxs-lookup"><span data-stu-id="036a4-116">This file contains the tables to support the core ASP.NET services like membership.</span></span>

<span data-ttu-id="036a4-117">Ahora podemos comenzar por implementar el proceso de pago.</span><span class="sxs-lookup"><span data-stu-id="036a4-117">Now we can begin implementing the checkout process.</span></span>

<span data-ttu-id="036a4-118">Empiece por crear una página caja.aspx.</span><span class="sxs-lookup"><span data-stu-id="036a4-118">Begin by creating a CheckOut.aspx page.</span></span>

<span data-ttu-id="036a4-119">La página caja.aspx solo debe estar disponible para los usuarios que han iniciado sesión, por lo que nos restringirá el acceso que ha iniciado sesión y redirigir los usuarios que no ha iniciado sesión en la página de inicio de sesión.</span><span class="sxs-lookup"><span data-stu-id="036a4-119">The CheckOut.aspx page should only be available to users who are logged in so we will restrict access to logged in users and redirect users who are not logged in to the LogIn page.</span></span>

<span data-ttu-id="036a4-120">Para ello, vamos a agregar lo siguiente a la sección de configuración del archivo web.config.</span><span class="sxs-lookup"><span data-stu-id="036a4-120">To do this we'll add the following to the configuration section of our web.config file.</span></span>

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample1.xml)]

<span data-ttu-id="036a4-121">La plantilla para las aplicaciones de formularios Web Forms de ASP.NET se ha agregado una sección de autenticación a nuestro archivo web.config y es necesario establecer la página de inicio de sesión predeterminada automáticamente.</span><span class="sxs-lookup"><span data-stu-id="036a4-121">The template for ASP.NET Web Forms applications automatically added an authentication section to our web.config file and established the default login page.</span></span>

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample2.xml)]

<span data-ttu-id="036a4-122">Debemos modificamos Login.aspx archivo de código subyacente para migrar un carro de compra anónimo cuando el usuario inicia sesión.</span><span class="sxs-lookup"><span data-stu-id="036a4-122">We must modify the Login.aspx code behind file to migrate an anonymous shopping cart when the user logs in.</span></span> <span data-ttu-id="036a4-123">Cambiar la página\_como se indica a continuación, el evento de carga.</span><span class="sxs-lookup"><span data-stu-id="036a4-123">Change the Page\_Load event as follows.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample3.cs)]

<span data-ttu-id="036a4-124">A continuación, agregue un controlador de eventos "LoggedIn" similar al siguiente para establecer el nombre de sesión para el usuario recién conectado y cambie el identificador de sesión temporal en el carro de la compra a del usuario llamando al método MigrateCart en nuestra clase MyShoppingCart.</span><span class="sxs-lookup"><span data-stu-id="036a4-124">Then add a "LoggedIn" event handler like this to set the session name to the newly logged in user and change the temporary session id in the shopping cart to that of the user by calling the MigrateCart method in our MyShoppingCart class.</span></span> <span data-ttu-id="036a4-125">(Se implementa en el archivo. cs)</span><span class="sxs-lookup"><span data-stu-id="036a4-125">(Implemented in the .cs file)</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample4.cs)]

<span data-ttu-id="036a4-126">Implemente el método MigrateCart() como éste.</span><span class="sxs-lookup"><span data-stu-id="036a4-126">Implement the MigrateCart() method like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample5.cs)]

<span data-ttu-id="036a4-127">En caja.aspx usaremos un EntityDataSource y un control GridView en nuestra página retirar tanto como hicimos en nuestra página del carro de la compra.</span><span class="sxs-lookup"><span data-stu-id="036a4-127">In checkout.aspx we'll use an EntityDataSource and a GridView in our check out page much as we did in our shopping cart page.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-6/samples/sample6.aspx)]

<span data-ttu-id="036a4-128">Tenga en cuenta que nuestro control GridView especifica un controlador de eventos "ondatabound" denominado MyList\_RowDataBound así que vamos a implementar ese controlador de eventos similar al siguiente.</span><span class="sxs-lookup"><span data-stu-id="036a4-128">Note that our GridView control specifies an "ondatabound" event handler named MyList\_RowDataBound so let's implement that event handler like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample7.cs)]

<span data-ttu-id="036a4-129">Este mantiene método total de la compra el carro como cada fila está enlazada y actualiza la fila de la parte inferior del control GridView.</span><span class="sxs-lookup"><span data-stu-id="036a4-129">This method keeps a running total of the shopping cart as each row is bound and updates the bottom row of the GridView.</span></span>

<span data-ttu-id="036a4-130">Hemos implementado una presentación de "revisión" de la orden a colocarse en esta fase.</span><span class="sxs-lookup"><span data-stu-id="036a4-130">At this stage we have implemented a "review" presentation of the order to be placed.</span></span>

<span data-ttu-id="036a4-131">Vamos a controlar un escenario de carro vacío mediante la adición de unas pocas líneas de código a nuestra página\_eventos de carga:</span><span class="sxs-lookup"><span data-stu-id="036a4-131">Let's handle an empty cart scenario by adding a few lines of code to our Page\_Load event:</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample8.cs)]

<span data-ttu-id="036a4-132">Cuando el usuario hace clic en el botón "Enviar", se ejecutará el código siguiente en el controlador de envío de evento Click de botón.</span><span class="sxs-lookup"><span data-stu-id="036a4-132">When the user clicks on the "Submit" button we will execute the following code in the Submit Button Click Event handler.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample9.cs)]

<span data-ttu-id="036a4-133">Es la "carne" el proceso de envío de pedido se implementa en el método SubmitOrder() de nuestra clase MyShoppingCart.</span><span class="sxs-lookup"><span data-stu-id="036a4-133">The "meat" of the order submission process is to be implemented in the SubmitOrder() method of our MyShoppingCart class.</span></span>

<span data-ttu-id="036a4-134">SubmitOrder hará lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="036a4-134">SubmitOrder will:</span></span>

- <span data-ttu-id="036a4-135">Tomar todos los elementos de línea en el carro de la compra y usarlos para crear un nuevo registro de pedido y los registros de OrderDetails asociados.</span><span class="sxs-lookup"><span data-stu-id="036a4-135">Take all the line items in the shopping cart and use them to create a new Order Record and the associated OrderDetails records.</span></span>
- <span data-ttu-id="036a4-136">Calcular la fecha de envío.</span><span class="sxs-lookup"><span data-stu-id="036a4-136">Calculate Shipping Date.</span></span>
- <span data-ttu-id="036a4-137">Desactive el carro de la compra.</span><span class="sxs-lookup"><span data-stu-id="036a4-137">Clear the shopping cart.</span></span>


[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample10.cs)]

<span data-ttu-id="036a4-138">Para los fines de esta aplicación de ejemplo se a calcular una fecha de envío, agregando dos días hasta la fecha actual.</span><span class="sxs-lookup"><span data-stu-id="036a4-138">For the purposes of this sample application we'll calculate a ship date by simply adding two days to the current date.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample11.cs)]

<span data-ttu-id="036a4-139">Ejecutar la aplicación ahora nos permitirá probar el proceso de compra de principio a fin.</span><span class="sxs-lookup"><span data-stu-id="036a4-139">Running the application now will permit us to test the shopping process from start to finish.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="036a4-140">[Anterior](tailspin-spyworks-part-5.md)
> [Siguiente](tailspin-spyworks-part-7.md)</span><span class="sxs-lookup"><span data-stu-id="036a4-140">[Previous](tailspin-spyworks-part-5.md)
[Next](tailspin-spyworks-part-7.md)</span></span>
