---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/creating-an-odata-endpoint
title: Creación de un punto de conexión de OData v3 con Web API 2 | Microsoft Docs
author: MikeWasson
description: Open Data Protocol (OData) es un protocolo de acceso a datos para la web. OData proporciona una manera uniforme para estructurar datos, consultar los datos y manipular los datos...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/25/2014
ms.topic: article
ms.assetid: 262843d6-43a2-4f1c-82d9-0b90ae6df0cf
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/creating-an-odata-endpoint
msc.type: authoredcontent
ms.openlocfilehash: bcf089003cfb4233fd2aa70cf669972de46c27b1
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37362595"
---
<a name="creating-an-odata-v3-endpoint-with-web-api-2"></a><span data-ttu-id="c7d35-104">Creación de un punto de conexión de OData v3 con Web API 2</span><span class="sxs-lookup"><span data-stu-id="c7d35-104">Creating an OData v3 Endpoint with Web API 2</span></span>
====================
<span data-ttu-id="c7d35-105">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="c7d35-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="c7d35-106">Descargue el proyecto completado</span><span class="sxs-lookup"><span data-stu-id="c7d35-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> <span data-ttu-id="c7d35-107">El [Open Data Protocol](http://www.odata.org/) (OData) es un protocolo de acceso a datos para la web.</span><span class="sxs-lookup"><span data-stu-id="c7d35-107">The [Open Data Protocol](http://www.odata.org/) (OData) is a data access protocol for the web.</span></span> <span data-ttu-id="c7d35-108">OData proporciona una manera uniforme para estructurar datos, consultar los datos y manipular el conjunto de datos a través de las operaciones CRUD (crear, leer, actualizar y eliminar).</span><span class="sxs-lookup"><span data-stu-id="c7d35-108">OData provides a uniform way to structure data, query the data, and manipulate the data set through CRUD operations (create, read, update, and delete).</span></span> <span data-ttu-id="c7d35-109">OData admite formatos JSON y AtomPub (XML).</span><span class="sxs-lookup"><span data-stu-id="c7d35-109">OData supports both AtomPub (XML) and JSON formats.</span></span> <span data-ttu-id="c7d35-110">OData también define un método para exponer metadatos sobre los datos.</span><span class="sxs-lookup"><span data-stu-id="c7d35-110">OData also defines a way to expose metadata about the data.</span></span> <span data-ttu-id="c7d35-111">Los clientes pueden usar los metadatos para detectar la información de tipos y relaciones para el conjunto de datos.</span><span class="sxs-lookup"><span data-stu-id="c7d35-111">Clients can use the metadata to discover the type information and relationships for the data set.</span></span>
> 
> <span data-ttu-id="c7d35-112">ASP.NET Web API facilita la creación de un extremo de OData para un conjunto de datos.</span><span class="sxs-lookup"><span data-stu-id="c7d35-112">ASP.NET Web API makes it easy to create an OData endpoint for a data set.</span></span> <span data-ttu-id="c7d35-113">Puede controlar exactamente qué operaciones de OData en el extremo admite.</span><span class="sxs-lookup"><span data-stu-id="c7d35-113">You can control exactly which OData operations the endpoint supports.</span></span> <span data-ttu-id="c7d35-114">Puede hospedar varios puntos de conexión de OData, junto con los puntos de conexión no OData.</span><span class="sxs-lookup"><span data-stu-id="c7d35-114">You can host multiple OData endpoints, alongside non-OData endpoints.</span></span> <span data-ttu-id="c7d35-115">Tiene control total sobre su modelo de datos, la lógica de negocios de back-end y la capa de datos.</span><span class="sxs-lookup"><span data-stu-id="c7d35-115">You have full control over your data model, back-end business logic, and data layer.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="c7d35-116">Versiones de software que se usa en el tutorial</span><span class="sxs-lookup"><span data-stu-id="c7d35-116">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="c7d35-117">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="c7d35-117">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="c7d35-118">Web API 2</span><span class="sxs-lookup"><span data-stu-id="c7d35-118">Web API 2</span></span>
> - <span data-ttu-id="c7d35-119">OData versión 3</span><span class="sxs-lookup"><span data-stu-id="c7d35-119">OData Version 3</span></span>
> - <span data-ttu-id="c7d35-120">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="c7d35-120">Entity Framework 6</span></span>
> - [<span data-ttu-id="c7d35-121">(Opcional) de Proxy de depuración de Web de Fiddler</span><span class="sxs-lookup"><span data-stu-id="c7d35-121">Fiddler Web Debugging Proxy (Optional)</span></span>](http://www.fiddler2.com)
> 
> <span data-ttu-id="c7d35-122">Se agregó compatibilidad con la Web API OData en [ASP.NET y Web Tools 2012.2 Update](https://go.microsoft.com/fwlink/?LinkId=282650).</span><span class="sxs-lookup"><span data-stu-id="c7d35-122">Web API OData support was added in [ASP.NET and Web Tools 2012.2 Update](https://go.microsoft.com/fwlink/?LinkId=282650).</span></span> <span data-ttu-id="c7d35-123">Sin embargo, en este tutorial se usa scaffolding que se agregó en Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="c7d35-123">However, this tutorial uses scaffolding that was added in Visual Studio 2013.</span></span>


<span data-ttu-id="c7d35-124">En este tutorial, creará un extremo OData simple que los clientes pueden consultar.</span><span class="sxs-lookup"><span data-stu-id="c7d35-124">In this tutorial, you will create a simple OData endpoint that clients can query.</span></span> <span data-ttu-id="c7d35-125">También creará a un cliente de C# para el punto de conexión.</span><span class="sxs-lookup"><span data-stu-id="c7d35-125">You will also create a C# client for the endpoint.</span></span> <span data-ttu-id="c7d35-126">Después de completar este tutorial, el siguiente conjunto de tutoriales se muestra cómo agregar más funcionalidad, incluidas las relaciones de entidad, acciones, y seleccione Expandir $/ $.</span><span class="sxs-lookup"><span data-stu-id="c7d35-126">After you complete this tutorial, the next set of tutorials show how to add more functionality, including entity relations, actions, and $expand/$select.</span></span>

- [<span data-ttu-id="c7d35-127">Crear el proyecto de Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c7d35-127">Create the Visual Studio Project</span></span>](#create-project)
- [<span data-ttu-id="c7d35-128">Agregar un modelo de entidades</span><span class="sxs-lookup"><span data-stu-id="c7d35-128">Add an Entity Model</span></span>](#add-model)
- [<span data-ttu-id="c7d35-129">Agregar un controlador de OData</span><span class="sxs-lookup"><span data-stu-id="c7d35-129">Add an OData Controller</span></span>](#add-controller)
- [<span data-ttu-id="c7d35-130">Agregue el EDM y ruta</span><span class="sxs-lookup"><span data-stu-id="c7d35-130">Add the EDM and Route</span></span>](#edm)
- [<span data-ttu-id="c7d35-131">Valor de inicialización de la base de datos (opcional)</span><span class="sxs-lookup"><span data-stu-id="c7d35-131">Seed the Database (Optional)</span></span>](#seed-db)
- [<span data-ttu-id="c7d35-132">Explorar el extremo de OData</span><span class="sxs-lookup"><span data-stu-id="c7d35-132">Exploring the OData Endpoint</span></span>](#explore)
- [<span data-ttu-id="c7d35-133">Formatos de serialización de OData</span><span class="sxs-lookup"><span data-stu-id="c7d35-133">OData Serialization Formats</span></span>](#formats)

<a id="create-project"></a>
## <a name="create-the-visual-studio-project"></a><span data-ttu-id="c7d35-134">Crear el proyecto de Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c7d35-134">Create the Visual Studio Project</span></span>

<span data-ttu-id="c7d35-135">En este tutorial, creará un extremo de OData que admite operaciones CRUD básicas.</span><span class="sxs-lookup"><span data-stu-id="c7d35-135">In this tutorial, you will create an OData endpoint that supports basic CRUD operations.</span></span> <span data-ttu-id="c7d35-136">El punto de conexión expondrá un único recurso, una lista de productos.</span><span class="sxs-lookup"><span data-stu-id="c7d35-136">The endpoint will expose a single resource, a list of products.</span></span> <span data-ttu-id="c7d35-137">Los tutoriales posteriores agregarán más características.</span><span class="sxs-lookup"><span data-stu-id="c7d35-137">Later tutorials will add more features.</span></span>

<span data-ttu-id="c7d35-138">Inicie Visual Studio y seleccione **nuevo proyecto** desde la página de inicio.</span><span class="sxs-lookup"><span data-stu-id="c7d35-138">Start Visual Studio and select **New Project** from the Start page.</span></span> <span data-ttu-id="c7d35-139">O bien, en el **archivo** menú, seleccione **New** y, a continuación, **proyecto**.</span><span class="sxs-lookup"><span data-stu-id="c7d35-139">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="c7d35-140">En el **plantillas** panel, seleccione **plantillas instaladas** y expanda el nodo Visual C#.</span><span class="sxs-lookup"><span data-stu-id="c7d35-140">In the **Templates** pane, select **Installed Templates** and expand the Visual C# node.</span></span> <span data-ttu-id="c7d35-141">En **Visual C#**, seleccione **Web**.</span><span class="sxs-lookup"><span data-stu-id="c7d35-141">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="c7d35-142">Seleccione **la aplicación Web ASP.NET** plantilla.</span><span class="sxs-lookup"><span data-stu-id="c7d35-142">Select **the ASP.NET Web Application** template.</span></span>

![](creating-an-odata-endpoint/_static/image1.png)

<span data-ttu-id="c7d35-143">En el **nuevo proyecto ASP.NET** cuadro de diálogo, seleccione el **vacía** plantilla.</span><span class="sxs-lookup"><span data-stu-id="c7d35-143">In the **New ASP.NET Project** dialog, select the **Empty** template.</span></span> <span data-ttu-id="c7d35-144">Bajo &quot;agregar carpetas y referencias centrales para... &quot;, comprobar **API Web**.</span><span class="sxs-lookup"><span data-stu-id="c7d35-144">Under &quot;Add folders and core references for...&quot;, check **Web API**.</span></span> <span data-ttu-id="c7d35-145">Haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="c7d35-145">Click **OK**.</span></span>

![](creating-an-odata-endpoint/_static/image2.png)

<a id="add-model"></a>
## <a name="add-an-entity-model"></a><span data-ttu-id="c7d35-146">Agregar un modelo de entidades</span><span class="sxs-lookup"><span data-stu-id="c7d35-146">Add an Entity Model</span></span>

<span data-ttu-id="c7d35-147">Un *modelo* es un objeto que representa los datos de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c7d35-147">A *model* is an object that represents the data in your application.</span></span> <span data-ttu-id="c7d35-148">Para este tutorial, se necesita un modelo que representa un producto.</span><span class="sxs-lookup"><span data-stu-id="c7d35-148">For this tutorial, we need a model that represents a product.</span></span> <span data-ttu-id="c7d35-149">El modelo corresponde a nuestro tipo de entidad de OData.</span><span class="sxs-lookup"><span data-stu-id="c7d35-149">The model corresponds to our OData entity type.</span></span>

<span data-ttu-id="c7d35-150">En el Explorador de soluciones, haga clic en la carpeta Models.</span><span class="sxs-lookup"><span data-stu-id="c7d35-150">In Solution Explorer, right-click the Models folder.</span></span> <span data-ttu-id="c7d35-151">En el menú contextual, seleccione **agregar** , a continuación, seleccione **clase**.</span><span class="sxs-lookup"><span data-stu-id="c7d35-151">From the context menu, select **Add** then select **Class**.</span></span>

![](creating-an-odata-endpoint/_static/image3.png)

<span data-ttu-id="c7d35-152">En el **Agregar nuevo** elemento de cuadro de diálogo, asigne el nombre de la clase &quot;producto&quot;.</span><span class="sxs-lookup"><span data-stu-id="c7d35-152">In the **Add New** Item dialog, name the class &quot;Product&quot;.</span></span>

![](creating-an-odata-endpoint/_static/image4.png)

> [!NOTE]
> <span data-ttu-id="c7d35-153">Por convención, las clases de modelo se colocan en la carpeta Models.</span><span class="sxs-lookup"><span data-stu-id="c7d35-153">By convention, model classes are placed in the Models folder.</span></span> <span data-ttu-id="c7d35-154">No tiene que seguir esta convención en sus propios proyectos, pero vamos a usar para este tutorial.</span><span class="sxs-lookup"><span data-stu-id="c7d35-154">You don't have to follow this convention in your own projects, but we'll use it for this tutorial.</span></span>


<span data-ttu-id="c7d35-155">En el archivo Product.cs, agregue la siguiente definición de clase:</span><span class="sxs-lookup"><span data-stu-id="c7d35-155">In the Product.cs file, add the following class definition:</span></span>

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample1.cs)]

<span data-ttu-id="c7d35-156">La propiedad de Id. será la clave de entidad.</span><span class="sxs-lookup"><span data-stu-id="c7d35-156">The ID property will be the entity key.</span></span> <span data-ttu-id="c7d35-157">Los clientes pueden consultar los productos por identificador.</span><span class="sxs-lookup"><span data-stu-id="c7d35-157">Clients can query products by ID.</span></span> <span data-ttu-id="c7d35-158">Este campo también sería la clave principal de la base de datos back-end.</span><span class="sxs-lookup"><span data-stu-id="c7d35-158">This field would also be the primary key in the back-end database.</span></span>

<span data-ttu-id="c7d35-159">Compile el proyecto ahora.</span><span class="sxs-lookup"><span data-stu-id="c7d35-159">Build the project now.</span></span> <span data-ttu-id="c7d35-160">En el paso siguiente, vamos a usar algunos scaffolding de Visual Studio que usa la reflexión para encontrar el tipo de producto.</span><span class="sxs-lookup"><span data-stu-id="c7d35-160">In the next step, we'll use some Visual Studio scaffolding that uses reflection to find the Product type.</span></span>

<a id="add-controller"></a>
## <a name="add-an-odata-controller"></a><span data-ttu-id="c7d35-161">Agregar un controlador de OData</span><span class="sxs-lookup"><span data-stu-id="c7d35-161">Add an OData Controller</span></span>

<span data-ttu-id="c7d35-162">Un *controlador* es una clase que controla las solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="c7d35-162">A *controller* is a class that handles HTTP requests.</span></span> <span data-ttu-id="c7d35-163">Definir un controlador independiente para cada conjunto de entidades en que el servicio de OData.</span><span class="sxs-lookup"><span data-stu-id="c7d35-163">You define a separate controller for each entity set in you OData service.</span></span> <span data-ttu-id="c7d35-164">En este tutorial, vamos a crear un único controlador.</span><span class="sxs-lookup"><span data-stu-id="c7d35-164">In this tutorial, we'll create a single controller.</span></span>

<span data-ttu-id="c7d35-165">En el Explorador de soluciones, haga clic en la carpeta Controllers.</span><span class="sxs-lookup"><span data-stu-id="c7d35-165">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="c7d35-166">Seleccione **agregar** y, a continuación, seleccione **controlador**.</span><span class="sxs-lookup"><span data-stu-id="c7d35-166">Select **Add** and then select **Controller**.</span></span>

![](creating-an-odata-endpoint/_static/image5.png)

<span data-ttu-id="c7d35-167">En el **agregar Scaffold** cuadro de diálogo, seleccione &quot;controlador Web API 2 OData con acciones mediante Entity Framework&quot;.</span><span class="sxs-lookup"><span data-stu-id="c7d35-167">In the **Add Scaffold** dialog, select &quot;Web API 2 OData Controller with actions, using Entity Framework&quot;.</span></span>

![](creating-an-odata-endpoint/_static/image6.png)

<span data-ttu-id="c7d35-168">En el **Agregar controlador** cuadro de diálogo, el nombre del controlador "ProductsController".</span><span class="sxs-lookup"><span data-stu-id="c7d35-168">In the **Add Controller** dialog, name the controller "ProductsController".</span></span> <span data-ttu-id="c7d35-169">Seleccione el &quot;usar acciones de controlador asincrónicas&quot; casilla de verificación.</span><span class="sxs-lookup"><span data-stu-id="c7d35-169">Select the &quot;Use async controller actions&quot; checkbox.</span></span> <span data-ttu-id="c7d35-170">En el **modelo** lista desplegable, seleccione la clase de producto.</span><span class="sxs-lookup"><span data-stu-id="c7d35-170">In the **Model** drop-down list, select the Product class.</span></span>

![](creating-an-odata-endpoint/_static/image7.png)

<span data-ttu-id="c7d35-171">Haga clic en el **nuevo contexto de datos...**  botón.</span><span class="sxs-lookup"><span data-stu-id="c7d35-171">Click the **New data context...** button.</span></span> <span data-ttu-id="c7d35-172">Deje el nombre predeterminado para el tipo de contexto de datos y haga clic en **agregar**.</span><span class="sxs-lookup"><span data-stu-id="c7d35-172">Leave the default name for the data context type, and click **Add**.</span></span>

![](creating-an-odata-endpoint/_static/image8.png)

<span data-ttu-id="c7d35-173">Haga clic en Agregar en el cuadro de diálogo Agregar controlador para agregar el controlador.</span><span class="sxs-lookup"><span data-stu-id="c7d35-173">Click Add in the Add Controller dialog to add the controller.</span></span>

![](creating-an-odata-endpoint/_static/image9.png)

<span data-ttu-id="c7d35-174">Nota: Si recibe un mensaje de error que dice &quot;se produjo un error al obtener el tipo... &quot;, asegúrese de que ha generado el proyecto de Visual Studio después de agregar la clase de producto.</span><span class="sxs-lookup"><span data-stu-id="c7d35-174">Note: If you get an error message that says &quot;There was an error getting the type...&quot;, make sure that you built the Visual Studio project after you added the Product class.</span></span> <span data-ttu-id="c7d35-175">El scaffolding usa la reflexión para encontrar la clase.</span><span class="sxs-lookup"><span data-stu-id="c7d35-175">The scaffolding uses reflection to find the class.</span></span>

![](creating-an-odata-endpoint/_static/image10.png)

<span data-ttu-id="c7d35-176">El scaffolding agrega dos archivos de código al proyecto:</span><span class="sxs-lookup"><span data-stu-id="c7d35-176">The scaffolding adds two code files to the project:</span></span>

- <span data-ttu-id="c7d35-177">Products.cs define el controlador de API Web que implementa el extremo de OData.</span><span class="sxs-lookup"><span data-stu-id="c7d35-177">Products.cs defines the Web API controller that implements the OData endpoint.</span></span>
- <span data-ttu-id="c7d35-178">ProductServiceContext.cs proporciona métodos para consultar la base de datos subyacente mediante Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="c7d35-178">ProductServiceContext.cs provides methods to query the underlying database, using Entity Framework.</span></span>

![](creating-an-odata-endpoint/_static/image11.png)

<a id="edm"></a>
## <a name="add-the-edm-and-route"></a><span data-ttu-id="c7d35-179">Agregue el EDM y ruta</span><span class="sxs-lookup"><span data-stu-id="c7d35-179">Add the EDM and Route</span></span>

<span data-ttu-id="c7d35-180">En el Explorador de soluciones, expanda la aplicación\_carpeta Inicio y abra el archivo WebApiConfig.cs.</span><span class="sxs-lookup"><span data-stu-id="c7d35-180">In Solution Explorer, expand the App\_Start folder and open the file named WebApiConfig.cs.</span></span> <span data-ttu-id="c7d35-181">Esta clase contiene el código de configuración de Web API.</span><span class="sxs-lookup"><span data-stu-id="c7d35-181">This class holds configuration code for Web API.</span></span> <span data-ttu-id="c7d35-182">Reemplace este código con lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="c7d35-182">Replace this code with the following:</span></span>

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample2.cs)]

<span data-ttu-id="c7d35-183">Este código hace dos cosas:</span><span class="sxs-lookup"><span data-stu-id="c7d35-183">This code does two things:</span></span>

- <span data-ttu-id="c7d35-184">Crea un Entity Data Model (EDM) para el extremo de OData.</span><span class="sxs-lookup"><span data-stu-id="c7d35-184">Creates an Entity Data Model (EDM) for the OData endpoint.</span></span>
- <span data-ttu-id="c7d35-185">Agrega una ruta para el punto de conexión.</span><span class="sxs-lookup"><span data-stu-id="c7d35-185">Adds a route for the endpoint.</span></span>

<span data-ttu-id="c7d35-186">Un EDM es un modelo abstracto de los datos.</span><span class="sxs-lookup"><span data-stu-id="c7d35-186">An EDM is an abstract model of the data.</span></span> <span data-ttu-id="c7d35-187">El EDM se usa para crear el documento de metadatos y definir a los URI para el servicio.</span><span class="sxs-lookup"><span data-stu-id="c7d35-187">The EDM is used to create the metadata document and define the URIs for the service.</span></span> <span data-ttu-id="c7d35-188">El **ODataConventionModelBuilder** crea un modelo EDM utilizando un conjunto de convenciones de nomenclatura predeterminadas EDM.</span><span class="sxs-lookup"><span data-stu-id="c7d35-188">The **ODataConventionModelBuilder** creates an EDM by using a set of default naming conventions EDM.</span></span> <span data-ttu-id="c7d35-189">Este enfoque requiere menos código.</span><span class="sxs-lookup"><span data-stu-id="c7d35-189">This approach requires the least code.</span></span> <span data-ttu-id="c7d35-190">Si desea más control sobre el modelo EDM, puede usar el **ODataModelBuilder** clase para crear el EDM agregando explícitamente las propiedades, claves y las propiedades de navegación.</span><span class="sxs-lookup"><span data-stu-id="c7d35-190">If you want more control over the EDM, you can use the **ODataModelBuilder** class to create the EDM by adding properties, keys, and navigation properties explicitly.</span></span>

<span data-ttu-id="c7d35-191">El **EntitySet** método agrega un conjunto de entidades al EDM:</span><span class="sxs-lookup"><span data-stu-id="c7d35-191">The **EntitySet** method adds an entity set to the EDM:</span></span>

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample3.cs)]

<span data-ttu-id="c7d35-192">La cadena "Productos" define el nombre del conjunto de entidades.</span><span class="sxs-lookup"><span data-stu-id="c7d35-192">The string "Products" defines the name of the entity set.</span></span> <span data-ttu-id="c7d35-193">El nombre del controlador debe coincidir con el nombre del conjunto de entidades.</span><span class="sxs-lookup"><span data-stu-id="c7d35-193">The name of the controller must match the name of the entity set.</span></span> <span data-ttu-id="c7d35-194">En este tutorial, el conjunto de entidades se denomina "Productos" y el controlador se denomina `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="c7d35-194">In this tutorial, the entity set is named "Products" and the controller is named `ProductsController`.</span></span> <span data-ttu-id="c7d35-195">Si el nombre de conjunto de entidades "Conjunto de productos", podría denominar el controlador `ProductSetController`.</span><span class="sxs-lookup"><span data-stu-id="c7d35-195">If you named the entity set "ProductSet", you would name the controller `ProductSetController`.</span></span> <span data-ttu-id="c7d35-196">Tenga en cuenta que un punto de conexión puede tener varios conjuntos de entidades.</span><span class="sxs-lookup"><span data-stu-id="c7d35-196">Note that an endpoint can have multiple entity sets.</span></span> <span data-ttu-id="c7d35-197">Llame a **EntitySet&lt;T&gt;**  para cada entidad establecer y, a continuación, defina un controlador correspondiente.</span><span class="sxs-lookup"><span data-stu-id="c7d35-197">Call **EntitySet&lt;T&gt;** for each entity set, and then define a corresponding controller.</span></span>

<span data-ttu-id="c7d35-198">El **MapODataRoute** método agrega una ruta para el extremo de OData.</span><span class="sxs-lookup"><span data-stu-id="c7d35-198">The **MapODataRoute** method adds a route for the OData endpoint.</span></span>

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample4.cs)]

<span data-ttu-id="c7d35-199">El primer parámetro es un nombre descriptivo para la ruta.</span><span class="sxs-lookup"><span data-stu-id="c7d35-199">The first parameter is a friendly name for the route.</span></span> <span data-ttu-id="c7d35-200">Los clientes del servicio no ven este nombre.</span><span class="sxs-lookup"><span data-stu-id="c7d35-200">Clients of your service do not see this name.</span></span> <span data-ttu-id="c7d35-201">El segundo parámetro es el prefijo URI para el punto de conexión.</span><span class="sxs-lookup"><span data-stu-id="c7d35-201">The second parameter is the URI prefix for the endpoint.</span></span> <span data-ttu-id="c7d35-202">Dado que este código, el URI para el conjunto de entidades de productos es http://<em>hostname</em>  /odata o productos.</span><span class="sxs-lookup"><span data-stu-id="c7d35-202">Given this code, the URI for the Products entity set is http://<em>hostname</em>/odata/Products.</span></span> <span data-ttu-id="c7d35-203">La aplicación puede tener más de un extremo de OData.</span><span class="sxs-lookup"><span data-stu-id="c7d35-203">Your application can have more than one OData endpoint.</span></span> <span data-ttu-id="c7d35-204">Para cada punto de conexión, llame a <strong>MapODataRoute</strong> y proporcione un nombre de ruta único y un prefijo de identificador URI único.</span><span class="sxs-lookup"><span data-stu-id="c7d35-204">For each endpoint, call <strong>MapODataRoute</strong> and provide a unique route name and a unique URI prefix.</span></span>

<a id="seed-db"></a>
## <a name="seed-the-database-optional"></a><span data-ttu-id="c7d35-205">Valor de inicialización de la base de datos (opcional)</span><span class="sxs-lookup"><span data-stu-id="c7d35-205">Seed the Database (Optional)</span></span>

<span data-ttu-id="c7d35-206">En este paso, usará Entity Framework para inicializar la base de datos con algunos datos de prueba.</span><span class="sxs-lookup"><span data-stu-id="c7d35-206">In this step, you will use Entity Framework to seed the database with some test data.</span></span> <span data-ttu-id="c7d35-207">Este paso es opcional, pero le permite probar el punto de conexión de OData de inmediato.</span><span class="sxs-lookup"><span data-stu-id="c7d35-207">This step is optional, but it lets you test out your OData endpoint right away.</span></span>

<span data-ttu-id="c7d35-208">Desde el **herramientas** menú, seleccione **Administrador de paquetes de biblioteca**, a continuación, seleccione **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="c7d35-208">From the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="c7d35-209">En la ventana de consola de administrador de paquetes, escriba el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="c7d35-209">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample5.cmd)]

<span data-ttu-id="c7d35-210">Esto agrega una carpeta denominada migraciones y el archivo de código Configuration.cs.</span><span class="sxs-lookup"><span data-stu-id="c7d35-210">This adds a folder named Migrations and a code file named Configuration.cs.</span></span>

![](creating-an-odata-endpoint/_static/image12.png)

<span data-ttu-id="c7d35-211">Abra este archivo y agregue el código siguiente a la `Configuration.Seed` método.</span><span class="sxs-lookup"><span data-stu-id="c7d35-211">Open this file and add the following code to the `Configuration.Seed` method.</span></span>

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample6.cs)]

<span data-ttu-id="c7d35-212">En la ventana de consola de administrador de paquetes, escriba los siguientes comandos:</span><span class="sxs-lookup"><span data-stu-id="c7d35-212">In the Package Manager Console Window, enter the following commands:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample7.cmd)]

<span data-ttu-id="c7d35-213">Estos comandos generan código que crea la base de datos y, a continuación, ejecuta ese código.</span><span class="sxs-lookup"><span data-stu-id="c7d35-213">These commands generate code that creates the database, and then executes that code.</span></span>

<a id="explore"></a>
## <a name="exploring-the-odata-endpoint"></a><span data-ttu-id="c7d35-214">Explorar el extremo de OData</span><span class="sxs-lookup"><span data-stu-id="c7d35-214">Exploring the OData Endpoint</span></span>

<span data-ttu-id="c7d35-215">En esta sección, vamos a usar el [Fiddler Web Debugging Proxy](http://www.fiddler2.com) para enviar solicitudes al punto de conexión y examinar los mensajes de respuesta.</span><span class="sxs-lookup"><span data-stu-id="c7d35-215">In this section, we'll use the [Fiddler Web Debugging Proxy](http://www.fiddler2.com) to send requests to the endpoint and examine the response messages.</span></span> <span data-ttu-id="c7d35-216">Esto le ayudará a comprender las capacidades de un extremo de OData.</span><span class="sxs-lookup"><span data-stu-id="c7d35-216">This will help you to understand the capabilities of an OData endpoint.</span></span>

<span data-ttu-id="c7d35-217">En Visual Studio, presione F5 para iniciar la depuración.</span><span class="sxs-lookup"><span data-stu-id="c7d35-217">In Visual Studio, press F5 to start debugging.</span></span> <span data-ttu-id="c7d35-218">De forma predeterminada, Visual Studio abre el explorador en `http://localhost:*port*`, donde *puerto* es el número de puerto configurado en la configuración del proyecto.</span><span class="sxs-lookup"><span data-stu-id="c7d35-218">By default, Visual Studio opens your browser to `http://localhost:*port*`, where *port* is the port number configured in the project settings.</span></span>

<span data-ttu-id="c7d35-219">Puede cambiar el número de puerto en la configuración del proyecto.</span><span class="sxs-lookup"><span data-stu-id="c7d35-219">You can change the port number in the project settings.</span></span> <span data-ttu-id="c7d35-220">En el Explorador de soluciones, haga clic en el proyecto y seleccione **propiedades**.</span><span class="sxs-lookup"><span data-stu-id="c7d35-220">In Solution Explorer, right-click the project and select **Properties**.</span></span> <span data-ttu-id="c7d35-221">En la ventana Propiedades, seleccione **Web**.</span><span class="sxs-lookup"><span data-stu-id="c7d35-221">In the properties window, select **Web**.</span></span> <span data-ttu-id="c7d35-222">Escriba el número de puerto en **dirección Url del proyecto**.</span><span class="sxs-lookup"><span data-stu-id="c7d35-222">Enter the port number under **Project Url**.</span></span>

### <a name="service-document"></a><span data-ttu-id="c7d35-223">Documento de servicio</span><span class="sxs-lookup"><span data-stu-id="c7d35-223">Service Document</span></span>

<span data-ttu-id="c7d35-224">El *documento de servicio* contiene una lista de los conjuntos de entidades para el extremo de OData.</span><span class="sxs-lookup"><span data-stu-id="c7d35-224">The *service document* contains a list of the entity sets for the OData endpoint.</span></span> <span data-ttu-id="c7d35-225">Para obtener el documento de servicio, envía una solicitud GET al URI del servicio en la raíz.</span><span class="sxs-lookup"><span data-stu-id="c7d35-225">To get the service document, send a GET request to the root URI of the service.</span></span>

<span data-ttu-id="c7d35-226">Uso de Fiddler, escriba el siguiente URI en el **Composer** ficha: `http://localhost:port/odata/`, donde *puerto* es el número de puerto.</span><span class="sxs-lookup"><span data-stu-id="c7d35-226">Using Fiddler, enter the following URI in the **Composer** tab: `http://localhost:port/odata/`, where *port* is the port number.</span></span>

![](creating-an-odata-endpoint/_static/image13.png)

<span data-ttu-id="c7d35-227">Haga clic en el **Execute** botón.</span><span class="sxs-lookup"><span data-stu-id="c7d35-227">Click the **Execute** button.</span></span> <span data-ttu-id="c7d35-228">Fiddler envía una solicitud HTTP GET a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c7d35-228">Fiddler sends an HTTP GET request to your application.</span></span> <span data-ttu-id="c7d35-229">Debería ver la respuesta en la lista de sesiones de la Web.</span><span class="sxs-lookup"><span data-stu-id="c7d35-229">You should see the response in the Web Sessions list.</span></span> <span data-ttu-id="c7d35-230">Si todo funciona, el código de estado será de 200.</span><span class="sxs-lookup"><span data-stu-id="c7d35-230">If everything is working, the status code will be 200.</span></span>

![](creating-an-odata-endpoint/_static/image14.png)

<span data-ttu-id="c7d35-231">Haga doble clic en la respuesta de la lista de sesiones Web para ver los detalles del mensaje de respuesta en la pestaña inspectores.</span><span class="sxs-lookup"><span data-stu-id="c7d35-231">Double-click the response in the Web Sessions list to see the details of the response message in the Inspectors tab.</span></span>

![](creating-an-odata-endpoint/_static/image15.png)

<span data-ttu-id="c7d35-232">El mensaje de respuesta HTTP sin formato debe ser similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="c7d35-232">The raw HTTP response message should look similar to the following:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample8.cmd)]

<span data-ttu-id="c7d35-233">De forma predeterminada, API Web devuelve el documento de servicio en formato AtomPub.</span><span class="sxs-lookup"><span data-stu-id="c7d35-233">By default, Web API returns the service document in AtomPub format.</span></span> <span data-ttu-id="c7d35-234">Para solicitar JSON, agregue el siguiente encabezado a la solicitud HTTP:</span><span class="sxs-lookup"><span data-stu-id="c7d35-234">To request JSON, add the following header to the HTTP request:</span></span>

`Accept: application/json`

![](creating-an-odata-endpoint/_static/image16.png)

<span data-ttu-id="c7d35-235">Ahora, la respuesta HTTP contiene una carga JSON:</span><span class="sxs-lookup"><span data-stu-id="c7d35-235">Now the HTTP response contains a JSON payload:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample9.cmd)]

### <a name="service-metadata-document"></a><span data-ttu-id="c7d35-236">Documento de metadatos de servicio</span><span class="sxs-lookup"><span data-stu-id="c7d35-236">Service Metadata Document</span></span>

<span data-ttu-id="c7d35-237">El *documento de metadatos del servicio* describe el modelo de datos del servicio, mediante un lenguaje XML llamado el lenguaje de definición de esquemas conceptuales (CSDL).</span><span class="sxs-lookup"><span data-stu-id="c7d35-237">The *service metadata document* describes the data model of the service, using an XML language called the Conceptual Schema Definition Language (CSDL).</span></span> <span data-ttu-id="c7d35-238">El documento de metadatos muestra la estructura de los datos en el servicio y puede usarse para generar código de cliente.</span><span class="sxs-lookup"><span data-stu-id="c7d35-238">The metadata document shows the structure of the data in the service, and can be used to generate client code.</span></span>

<span data-ttu-id="c7d35-239">Para obtener el documento de metadatos, envíe una solicitud GET a `http://localhost:port/odata/$metadata`.</span><span class="sxs-lookup"><span data-stu-id="c7d35-239">To get the metadata document, send a GET request to `http://localhost:port/odata/$metadata`.</span></span> <span data-ttu-id="c7d35-240">Estos los metadatos para el extremo que se muestra en este tutorial.</span><span class="sxs-lookup"><span data-stu-id="c7d35-240">Here is the metadata for the endpoint shown in this tutorial.</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample10.cmd)]

### <a name="entity-set"></a><span data-ttu-id="c7d35-241">Conjunto de entidades</span><span class="sxs-lookup"><span data-stu-id="c7d35-241">Entity Set</span></span>

<span data-ttu-id="c7d35-242">Para obtener el conjunto de entidades de productos, envíe una solicitud GET a `http://localhost:port/odata/Products`.</span><span class="sxs-lookup"><span data-stu-id="c7d35-242">To get the Products entity set, send a GET request to `http://localhost:port/odata/Products`.</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample11.cmd)]

### <a name="entity"></a><span data-ttu-id="c7d35-243">Entity</span><span class="sxs-lookup"><span data-stu-id="c7d35-243">Entity</span></span>

<span data-ttu-id="c7d35-244">Para obtener un producto individual, envíe una solicitud GET a `http://localhost:port/odata/Products(1)`, donde "1" es el identificador de producto.</span><span class="sxs-lookup"><span data-stu-id="c7d35-244">To get an individual product, send a GET request to `http://localhost:port/odata/Products(1)`, where "1" is the product ID.</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample12.cmd)]

<a id="formats"></a>
## <a name="odata-serialization-formats"></a><span data-ttu-id="c7d35-245">Formatos de serialización de OData</span><span class="sxs-lookup"><span data-stu-id="c7d35-245">OData Serialization Formats</span></span>

<span data-ttu-id="c7d35-246">OData admite varios formatos de serialización:</span><span class="sxs-lookup"><span data-stu-id="c7d35-246">OData supports several serialization formats:</span></span>

- <span data-ttu-id="c7d35-247">Publicación de Atom (XML)</span><span class="sxs-lookup"><span data-stu-id="c7d35-247">Atom Pub (XML)</span></span>
- <span data-ttu-id="c7d35-248">JSON "light" (introducida en OData v3)</span><span class="sxs-lookup"><span data-stu-id="c7d35-248">JSON "light" (introduced in OData v3)</span></span>
- <span data-ttu-id="c7d35-249">JSON "detallado" (OData v2)</span><span class="sxs-lookup"><span data-stu-id="c7d35-249">JSON "verbose" (OData v2)</span></span>

<span data-ttu-id="c7d35-250">De forma predeterminada, API Web usa el formato de AtomPubJSON "light".</span><span class="sxs-lookup"><span data-stu-id="c7d35-250">By default, Web API uses AtomPubJSON "light" format.</span></span> 

<span data-ttu-id="c7d35-251">Para obtener el formato AtomPub, establezca el encabezado Accept "aplicación/Atom+XML".</span><span class="sxs-lookup"><span data-stu-id="c7d35-251">To get AtomPub format, set the Accept header to "application/atom+xml".</span></span> <span data-ttu-id="c7d35-252">Este es un cuerpo de respuesta de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="c7d35-252">Here is an example response body:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample13.cmd)]

<span data-ttu-id="c7d35-253">Puede ver una desventaja obvia del formato de Atom: es mucho más detallado que la luz JSON.</span><span class="sxs-lookup"><span data-stu-id="c7d35-253">You can see one obvious disadvantage of the Atom format: It's a lot more verbose than the JSON light.</span></span> <span data-ttu-id="c7d35-254">Sin embargo, si tiene un cliente que entiende AtomPub, el cliente puede preferir ese formato JSON.</span><span class="sxs-lookup"><span data-stu-id="c7d35-254">However, if you have a client that understands AtomPub, the client might prefer that format over JSON.</span></span>

<span data-ttu-id="c7d35-255">Esta es la versión ligera de JSON de la misma entidad:</span><span class="sxs-lookup"><span data-stu-id="c7d35-255">Here is the JSON light version of the same entity:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample14.cmd)]

<span data-ttu-id="c7d35-256">El formato JSON light se introdujo en la versión 3 del Protocolo OData.</span><span class="sxs-lookup"><span data-stu-id="c7d35-256">The JSON light format was introduced in version 3 of the OData protocol.</span></span> <span data-ttu-id="c7d35-257">Por compatibilidad con versiones anteriores, un cliente puede solicitar el formato JSON "detallado" anterior.</span><span class="sxs-lookup"><span data-stu-id="c7d35-257">For backward compatibility, a client can request the older "verbose" JSON format.</span></span> <span data-ttu-id="c7d35-258">Para solicitar JSON detallado, establezca el encabezado Accept en `application/json;odata=verbose`.</span><span class="sxs-lookup"><span data-stu-id="c7d35-258">To request verbose JSON, set the Accept header to `application/json;odata=verbose`.</span></span> <span data-ttu-id="c7d35-259">Esta es la versión detallada:</span><span class="sxs-lookup"><span data-stu-id="c7d35-259">Here is the verbose version:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample15.cmd)]

<span data-ttu-id="c7d35-260">Este formato transmite más metadatos en el cuerpo de respuesta, que puede agregar una sobrecarga considerable a través de una sesión completa.</span><span class="sxs-lookup"><span data-stu-id="c7d35-260">This format conveys more metadata in the response body, which can add considerable overhead over an entire session.</span></span> <span data-ttu-id="c7d35-261">Además, agrega un nivel de direccionamiento indirecto ajustando el objeto en una propiedad denominada "d".</span><span class="sxs-lookup"><span data-stu-id="c7d35-261">Also, it adds a level of indirection by wrapping the object in a property named "d".</span></span>

## <a name="next-steps"></a><span data-ttu-id="c7d35-262">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="c7d35-262">Next Steps</span></span>

- [<span data-ttu-id="c7d35-263">Agregar las relaciones de entidad</span><span class="sxs-lookup"><span data-stu-id="c7d35-263">Add Entity Relations</span></span>](working-with-entity-relations.md)
- [<span data-ttu-id="c7d35-264">Agregar acciones de OData</span><span class="sxs-lookup"><span data-stu-id="c7d35-264">Add OData Actions</span></span>](odata-actions.md)
- [<span data-ttu-id="c7d35-265">Llame al servicio de OData desde un cliente .NET</span><span class="sxs-lookup"><span data-stu-id="c7d35-265">Call the OData Service From a .NET Client</span></span>](calling-an-odata-service-from-a-net-client.md)
