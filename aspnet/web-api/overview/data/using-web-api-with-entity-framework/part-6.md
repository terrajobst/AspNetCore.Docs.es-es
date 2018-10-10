---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-6
title: Crear el cliente de JavaScript | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 20360326-b123-4b1e-abae-1d350edf4ce4
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-6
msc.type: authoredcontent
ms.openlocfilehash: b5cb4d93c30ef80a48da48ffc51dd51411b1d0d0
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912671"
---
<a name="create-the-javascript-client"></a><span data-ttu-id="8cb7e-102">Crear al cliente de JavaScript</span><span class="sxs-lookup"><span data-stu-id="8cb7e-102">Create the JavaScript Client</span></span>
====================
<span data-ttu-id="8cb7e-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="8cb7e-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="8cb7e-104">Descargue el proyecto completado</span><span class="sxs-lookup"><span data-stu-id="8cb7e-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="8cb7e-105">En esta sección, creará el cliente para la aplicación, mediante HTML, JavaScript y el [Knockout.js](http://knockoutjs.com/) biblioteca.</span><span class="sxs-lookup"><span data-stu-id="8cb7e-105">In this section, you will create the client for the application, using HTML, JavaScript, and the [Knockout.js](http://knockoutjs.com/) library.</span></span> <span data-ttu-id="8cb7e-106">Vamos a crear la aplicación cliente en fases:</span><span class="sxs-lookup"><span data-stu-id="8cb7e-106">We'll build the client app in stages:</span></span>

- <span data-ttu-id="8cb7e-107">Muestra una lista de los libros en pantalla.</span><span class="sxs-lookup"><span data-stu-id="8cb7e-107">Showing a list of books.</span></span>
- <span data-ttu-id="8cb7e-108">Mostrar detalles de un libro.</span><span class="sxs-lookup"><span data-stu-id="8cb7e-108">Showing a book detail.</span></span>
- <span data-ttu-id="8cb7e-109">Agregar un nuevo libro.</span><span class="sxs-lookup"><span data-stu-id="8cb7e-109">Adding a new book.</span></span>

<span data-ttu-id="8cb7e-110">La biblioteca Knockout usa el patrón Model-View-ViewModel (MVVM):</span><span class="sxs-lookup"><span data-stu-id="8cb7e-110">The Knockout library uses the Model-View-ViewModel (MVVM) pattern:</span></span>

- <span data-ttu-id="8cb7e-111">El **modelo** es la representación del lado servidor de los datos en el dominio de negocio (en nuestro caso, libros y autores).</span><span class="sxs-lookup"><span data-stu-id="8cb7e-111">The **model** is the server-side representation of the data in the business domain (in our case, books and authors).</span></span>
- <span data-ttu-id="8cb7e-112">El **vista** es la capa de presentación (HTML).</span><span class="sxs-lookup"><span data-stu-id="8cb7e-112">The **view** is the presentation layer (HTML).</span></span>
- <span data-ttu-id="8cb7e-113">El **modelo de vista** es un objeto de JavaScript que contiene los modelos.</span><span class="sxs-lookup"><span data-stu-id="8cb7e-113">The **view model** is a JavaScript object that holds the models.</span></span> <span data-ttu-id="8cb7e-114">El modelo de vista es una abstracción de código de la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="8cb7e-114">The view model is a code abstraction of the UI.</span></span> <span data-ttu-id="8cb7e-115">No tiene ningún conocimiento de la representación de HTML.</span><span class="sxs-lookup"><span data-stu-id="8cb7e-115">It has no knowledge of the HTML representation.</span></span> <span data-ttu-id="8cb7e-116">En su lugar, características abstractas de la vista, representa como &quot;una lista de libros&quot;.</span><span class="sxs-lookup"><span data-stu-id="8cb7e-116">Instead, it represents abstract features of the view, such as &quot;a list of books&quot;.</span></span>

<span data-ttu-id="8cb7e-117">La vista está enlazada a datos al modelo de vista.</span><span class="sxs-lookup"><span data-stu-id="8cb7e-117">The view is data-bound to the view model.</span></span> <span data-ttu-id="8cb7e-118">Actualizaciones para el modelo de vista se reflejan automáticamente en la vista.</span><span class="sxs-lookup"><span data-stu-id="8cb7e-118">Updates to the view model are automatically reflected in the view.</span></span> <span data-ttu-id="8cb7e-119">El modelo de vista también obtiene los eventos de la vista, como clics de botón.</span><span class="sxs-lookup"><span data-stu-id="8cb7e-119">The view model also gets events from the view, such as button clicks.</span></span>

![](part-6/_static/image1.png)

<span data-ttu-id="8cb7e-120">Este enfoque facilita cambiar el diseño y la interfaz de usuario de la aplicación, ya que puede cambiar los enlaces, sin tener que reescribir código.</span><span class="sxs-lookup"><span data-stu-id="8cb7e-120">This approach makes it easy to change the layout and UI of your app, because you can change the bindings, without rewriting any code.</span></span> <span data-ttu-id="8cb7e-121">Por ejemplo, podría mostrar una lista de elementos como un `<ul>`, cambiarlo más adelante a una tabla.</span><span class="sxs-lookup"><span data-stu-id="8cb7e-121">For example, you might show a list of items as a `<ul>`, then change it later to a table.</span></span>

## <a name="add-the-knockout-library"></a><span data-ttu-id="8cb7e-122">Agregue la biblioteca Knockout</span><span class="sxs-lookup"><span data-stu-id="8cb7e-122">Add the Knockout Library</span></span>

<span data-ttu-id="8cb7e-123">En Visual Studio, desde el **herramientas** menú, seleccione **Administrador de paquetes de NuGet**.</span><span class="sxs-lookup"><span data-stu-id="8cb7e-123">In Visual Studio, from the **Tools** menu, select **NuGet Package Manager**.</span></span> <span data-ttu-id="8cb7e-124">A continuación, seleccione **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="8cb7e-124">Then select **Package Manager Console**.</span></span> <span data-ttu-id="8cb7e-125">En la ventana de consola de administrador de paquetes, escriba el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="8cb7e-125">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](part-6/samples/sample1.cmd)]

<span data-ttu-id="8cb7e-126">Este comando agrega los archivos de Knockout en la carpeta de Scripts.</span><span class="sxs-lookup"><span data-stu-id="8cb7e-126">This command adds the Knockout files to the Scripts folder.</span></span>

## <a name="create-the-view-model"></a><span data-ttu-id="8cb7e-127">Crear el modelo de vista</span><span class="sxs-lookup"><span data-stu-id="8cb7e-127">Create the View Model</span></span>

<span data-ttu-id="8cb7e-128">Agregue un archivo JavaScript denominado app.js a la carpeta Scripts.</span><span class="sxs-lookup"><span data-stu-id="8cb7e-128">Add a JavaScript file named app.js to the Scripts folder.</span></span> <span data-ttu-id="8cb7e-129">(En el Explorador de soluciones, haga clic en la carpeta de Scripts, seleccione **agregar**, a continuación, seleccione **archivo JavaScript**.) Pegue el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="8cb7e-129">(In Solution Explorer, right-click the Scripts folder, select **Add**, then select **JavaScript File**.) Paste in the following code:</span></span>

[!code-javascript[Main](part-6/samples/sample2.js)]

<span data-ttu-id="8cb7e-130">En Knockout, la `observable` clase permite el enlace de datos.</span><span class="sxs-lookup"><span data-stu-id="8cb7e-130">In Knockout, the `observable` class enables data-binding.</span></span> <span data-ttu-id="8cb7e-131">Cuando se cambia el contenido de un objeto observable, el objeto observable notifica a todos los controles enlazados a datos, por lo que pueden actualizar a sí mismos.</span><span class="sxs-lookup"><span data-stu-id="8cb7e-131">When the contents of an observable change, the observable notifies all of the data-bound controls, so they can update themselves.</span></span> <span data-ttu-id="8cb7e-132">(El `observableArray` clase es la versión de la matriz de *observable*.) Para empezar, nuestro modelo de vista tiene dos objetos observables:</span><span class="sxs-lookup"><span data-stu-id="8cb7e-132">(The `observableArray` class is the array version of *observable*.) To start with, our view model has two observables:</span></span>

- <span data-ttu-id="8cb7e-133">`books` contiene la lista de los libros en pantalla.</span><span class="sxs-lookup"><span data-stu-id="8cb7e-133">`books` holds the list of books.</span></span>
- <span data-ttu-id="8cb7e-134">`error` contiene un mensaje de error si se produce un error en una llamada AJAX.</span><span class="sxs-lookup"><span data-stu-id="8cb7e-134">`error` contains an error message if an AJAX call fails.</span></span>

<span data-ttu-id="8cb7e-135">El `getAllBooks` método realiza una llamada AJAX al obtener la lista de los libros en pantalla.</span><span class="sxs-lookup"><span data-stu-id="8cb7e-135">The `getAllBooks` method makes an AJAX call to get the list of books.</span></span> <span data-ttu-id="8cb7e-136">A continuación, inserta el resultado en el `books` matriz.</span><span class="sxs-lookup"><span data-stu-id="8cb7e-136">Then it pushes the result onto the `books` array.</span></span>

<span data-ttu-id="8cb7e-137">El `ko.applyBindings` método forma parte de la biblioteca Knockout.</span><span class="sxs-lookup"><span data-stu-id="8cb7e-137">The `ko.applyBindings` method is part of the Knockout library.</span></span> <span data-ttu-id="8cb7e-138">Toma el modelo de vista como un parámetro y establece el enlace de datos.</span><span class="sxs-lookup"><span data-stu-id="8cb7e-138">It takes the view model as a parameter and sets up the data binding.</span></span>

## <a name="add-a-script-bundle"></a><span data-ttu-id="8cb7e-139">Agregar un paquete de scripts</span><span class="sxs-lookup"><span data-stu-id="8cb7e-139">Add a Script Bundle</span></span>

<span data-ttu-id="8cb7e-140">La unión es una característica de ASP.NET 4.5 que facilita combinar o agrupar varios archivos en un único archivo.</span><span class="sxs-lookup"><span data-stu-id="8cb7e-140">Bundling is a feature in ASP.NET 4.5 that makes it easy to combine or bundle multiple files into a single file.</span></span> <span data-ttu-id="8cb7e-141">La unión reduce el número de solicitudes al servidor, lo que puede mejorar el tiempo de carga de página.</span><span class="sxs-lookup"><span data-stu-id="8cb7e-141">Bundling reduces the number of requests to the server, which can improve page load time.</span></span>

<span data-ttu-id="8cb7e-142">Abra el archivo de aplicación\_Start/BundleConfig.cs.</span><span class="sxs-lookup"><span data-stu-id="8cb7e-142">Open the file App\_Start/BundleConfig.cs.</span></span> <span data-ttu-id="8cb7e-143">Agregue el código siguiente al método RegisterBundles.</span><span class="sxs-lookup"><span data-stu-id="8cb7e-143">Add the following code to the RegisterBundles method.</span></span>

[!code-csharp[Main](part-6/samples/sample3.cs)]

> [!div class="step-by-step"]
> <span data-ttu-id="8cb7e-144">[Anterior](part-5.md)
> [Siguiente](part-7.md)</span><span class="sxs-lookup"><span data-stu-id="8cb7e-144">[Previous](part-5.md)
[Next](part-7.md)</span></span>
