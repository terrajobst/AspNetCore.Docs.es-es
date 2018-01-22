---
title: "Creación de servicios back-end para aplicaciones móviles nativas"
author: ardalis
description: 
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mobile/native-mobile-backend
ms.openlocfilehash: 0737cb1c81b6a143090234f37e5567d5c605b99e
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/19/2018
---
# <a name="creating-backend-services-for-native-mobile-applications"></a><span data-ttu-id="0ce73-102">Creación de servicios back-end para aplicaciones móviles nativas</span><span class="sxs-lookup"><span data-stu-id="0ce73-102">Creating Backend Services for Native Mobile Applications</span></span>

<span data-ttu-id="0ce73-103">Por [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="0ce73-103">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="0ce73-104">Aplicaciones móviles pueden comunicarse fácilmente con servicios back-end de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0ce73-104">Mobile apps can easily communicate with ASP.NET Core backend services.</span></span>

[<span data-ttu-id="0ce73-105">Ver o descargar código servicios back-end</span><span class="sxs-lookup"><span data-stu-id="0ce73-105">View or download sample backend services code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/mobile/native-mobile-backend/sample)

## <a name="the-sample-native-mobile-app"></a><span data-ttu-id="0ce73-106">El ejemplo de aplicación nativa de móvil</span><span class="sxs-lookup"><span data-stu-id="0ce73-106">The Sample Native Mobile App</span></span>

<span data-ttu-id="0ce73-107">Este tutorial muestra cómo crear servicios back-end mediante ASP.NET Core MVC para admitir aplicaciones móviles nativas.</span><span class="sxs-lookup"><span data-stu-id="0ce73-107">This tutorial demonstrates how to create backend services using ASP.NET Core MVC to support native mobile apps.</span></span> <span data-ttu-id="0ce73-108">Usa el [aplicación Xamarin Forms ToDoRest](https://developer.xamarin.com/guides/xamarin-forms/web-services/consuming/rest/) como su cliente nativo, lo que incluye clientes nativos independientes para dispositivos Android, iOS, universales de Windows y Windows Phone.</span><span class="sxs-lookup"><span data-stu-id="0ce73-108">It uses the [Xamarin Forms ToDoRest app](https://developer.xamarin.com/guides/xamarin-forms/web-services/consuming/rest/) as its native client, which includes separate native clients for Android, iOS, Windows Universal, and Window Phone devices.</span></span> <span data-ttu-id="0ce73-109">Puede seguir el tutorial vinculado para crear la aplicación nativa (e instalar las herramientas de Xamarin libres necesarias), así como descargar la solución de ejemplo de Xamarin.</span><span class="sxs-lookup"><span data-stu-id="0ce73-109">You can follow the linked tutorial to create the native app (and install the necessary free Xamarin tools), as well as download the Xamarin sample solution.</span></span> <span data-ttu-id="0ce73-110">El ejemplo de Xamarin incluye un proyecto de servicios de ASP.NET Web API 2, que sustituye a aplicaciones de ASP.NET Core de este artículo (con sin cambios requeridos por el cliente).</span><span class="sxs-lookup"><span data-stu-id="0ce73-110">The Xamarin sample includes an ASP.NET Web API 2 services project, which this article's ASP.NET Core app replaces (with no changes required by the client).</span></span>

![Para la aplicación realice Rest que se ejecuta en un smartphone Android](native-mobile-backend/_static/todo-android.png)

### <a name="features"></a><span data-ttu-id="0ce73-112">Características</span><span class="sxs-lookup"><span data-stu-id="0ce73-112">Features</span></span>

<span data-ttu-id="0ce73-113">Es compatible con la aplicación ToDoRest enumerar, agregar, eliminar y actualizar elementos pendientes.</span><span class="sxs-lookup"><span data-stu-id="0ce73-113">The ToDoRest app supports listing, adding, deleting, and updating To-Do items.</span></span> <span data-ttu-id="0ce73-114">Cada elemento tiene un identificador, un nombre, notas y una propiedad que indica si se ha realizado todavía.</span><span class="sxs-lookup"><span data-stu-id="0ce73-114">Each item has an ID, a Name, Notes, and a property indicating whether it's been Done yet.</span></span>

<span data-ttu-id="0ce73-115">La vista principal de los elementos, como se muestra arriba, muestra el nombre de cada elemento e indica si se realiza con una marca de verificación.</span><span class="sxs-lookup"><span data-stu-id="0ce73-115">The main view of the items, as shown above, lists each item's name and indicates if it is done with a checkmark.</span></span>

<span data-ttu-id="0ce73-116">Al puntear en el `+` icono abre un cuadro de diálogo Agregar elemento:</span><span class="sxs-lookup"><span data-stu-id="0ce73-116">Tapping the `+` icon opens an add item dialog:</span></span>

![Agregar cuadro de diálogo elemento](native-mobile-backend/_static/todo-android-new-item.png)

<span data-ttu-id="0ce73-118">Al puntear en un elemento en la pantalla principal de lista, se abre un cuadro de diálogo Editar, donde se puede modificar el nombre del elemento, notas y configuración listo, o se puede eliminar el elemento:</span><span class="sxs-lookup"><span data-stu-id="0ce73-118">Tapping an item on the main list screen opens up an edit dialog where the item's Name, Notes, and Done settings can be modified, or the item can be deleted:</span></span>

![Editar cuadro de diálogo elemento](native-mobile-backend/_static/todo-android-edit-item.png)

<span data-ttu-id="0ce73-120">En este ejemplo se configura de forma predeterminada para utilizar servicios de back-end alojados en developer.xamarin.com, lo que permite operaciones de solo lectura.</span><span class="sxs-lookup"><span data-stu-id="0ce73-120">This sample is configured by default to use backend services hosted at developer.xamarin.com, which allow read-only operations.</span></span> <span data-ttu-id="0ce73-121">Para probarlo usted mismo con la aplicación de ASP.NET Core que creó en la sección siguiente que se ejecutan en el equipo, debe actualizar la aplicación `RestUrl` constante.</span><span class="sxs-lookup"><span data-stu-id="0ce73-121">To test it out yourself against the ASP.NET Core app created in the next section running on your computer, you'll need to update the app's `RestUrl` constant.</span></span> <span data-ttu-id="0ce73-122">Navegue hasta la `ToDoREST` proyecto y abra el *Constants.cs* archivo.</span><span class="sxs-lookup"><span data-stu-id="0ce73-122">Navigate to the `ToDoREST` project and open the *Constants.cs* file.</span></span> <span data-ttu-id="0ce73-123">Reemplace la `RestUrl` con una dirección URL que incluye IP su equipo dirección (no localhost ni 127.0.0.1, puesto que esta dirección se utiliza desde el emulador de dispositivo, no desde el equipo).</span><span class="sxs-lookup"><span data-stu-id="0ce73-123">Replace the `RestUrl` with a URL that includes your machine's IP address (not localhost or 127.0.0.1, since this address is used from the device emulator, not from your machine).</span></span> <span data-ttu-id="0ce73-124">Incluya también el número de puerto (5000).</span><span class="sxs-lookup"><span data-stu-id="0ce73-124">Include the port number as well (5000).</span></span> <span data-ttu-id="0ce73-125">Para comprobar que los servicios funcionan con un dispositivo, asegúrese de que no tiene un active firewall bloqueando el acceso a este puerto.</span><span class="sxs-lookup"><span data-stu-id="0ce73-125">In order to test that your services work with a device, ensure you don't have an active firewall blocking access to this port.</span></span>

```csharp
// URL of REST service (Xamarin ReadOnly Service)
//public static string RestUrl = "http://developer.xamarin.com:8081/api/todoitems{0}";

// use your machine's IP address
public static string RestUrl = "http://192.168.1.207:5000/api/todoitems/{0}";
```

## <a name="creating-the-aspnet-core-project"></a><span data-ttu-id="0ce73-126">Crear el proyecto de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0ce73-126">Creating the ASP.NET Core Project</span></span>

<span data-ttu-id="0ce73-127">Crear una nueva aplicación Web de ASP.NET Core en Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="0ce73-127">Create a new ASP.NET Core Web Application in Visual Studio.</span></span> <span data-ttu-id="0ce73-128">Elija la plantilla de API Web y sin autenticación.</span><span class="sxs-lookup"><span data-stu-id="0ce73-128">Choose the Web API template and No Authentication.</span></span> <span data-ttu-id="0ce73-129">Denomine el proyecto *ToDoApi*.</span><span class="sxs-lookup"><span data-stu-id="0ce73-129">Name the project *ToDoApi*.</span></span>

![Cuadro de diálogo nueva aplicación Web ASP.NET con plantilla de proyecto de API Web seleccionado](native-mobile-backend/_static/web-api-template.png)

<span data-ttu-id="0ce73-131">La aplicación debe responder a todas las solicitudes realizadas al puerto 5000.</span><span class="sxs-lookup"><span data-stu-id="0ce73-131">The application should respond to all requests made to port 5000.</span></span> <span data-ttu-id="0ce73-132">Actualización *Program.cs* para incluir `.UseUrls("http://*:5000")` para lograr esto:</span><span class="sxs-lookup"><span data-stu-id="0ce73-132">Update *Program.cs* to include `.UseUrls("http://*:5000")` to achieve this:</span></span>

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Program.cs?range=10-16&highlight=3)]

> [!NOTE]
> <span data-ttu-id="0ce73-133">Asegúrese de que se ejecute la aplicación directamente, en lugar de detrás de IIS Express, que omite las solicitudes no locales de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="0ce73-133">Make sure you run the application directly, rather than behind IIS Express, which ignores non-local requests by default.</span></span> <span data-ttu-id="0ce73-134">Ejecute `dotnet run` desde un símbolo del sistema, o elija el perfil de nombre de aplicación en la lista desplegable de destino de depuración en la barra de herramientas de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="0ce73-134">Run `dotnet run` from a command prompt, or choose the application name profile from the Debug Target dropdown in the Visual Studio toolbar.</span></span>

<span data-ttu-id="0ce73-135">Agregar una clase de modelo para representar elementos pendientes.</span><span class="sxs-lookup"><span data-stu-id="0ce73-135">Add a model class to represent To-Do items.</span></span> <span data-ttu-id="0ce73-136">Marca requiere campos mediante el `[Required]` atributo:</span><span class="sxs-lookup"><span data-stu-id="0ce73-136">Mark required fields using the `[Required]` attribute:</span></span>

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Models/ToDoItem.cs)]

<span data-ttu-id="0ce73-137">Los métodos de API requieren una manera de trabajar con datos.</span><span class="sxs-lookup"><span data-stu-id="0ce73-137">The API methods require some way to work with data.</span></span> <span data-ttu-id="0ce73-138">Use el mismo `IToDoRepository` interfaz los usos de ejemplo originales de Xamarin:</span><span class="sxs-lookup"><span data-stu-id="0ce73-138">Use the same `IToDoRepository` interface the original Xamarin sample uses:</span></span>

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Interfaces/IToDoRepository.cs)]

<span data-ttu-id="0ce73-139">En este ejemplo, la implementación usa solo una colección de elementos privada:</span><span class="sxs-lookup"><span data-stu-id="0ce73-139">For this sample, the implementation just uses a private collection of items:</span></span>

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Services/ToDoRepository.cs)]

<span data-ttu-id="0ce73-140">Configurar la implementación en *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="0ce73-140">Configure the implementation in *Startup.cs*:</span></span>

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Startup.cs?highlight=6&range=29-35)]

<span data-ttu-id="0ce73-141">En este punto, está listo para crear el *ToDoItemsController*.</span><span class="sxs-lookup"><span data-stu-id="0ce73-141">At this point, you're ready to create the *ToDoItemsController*.</span></span>

> [!TIP]
> <span data-ttu-id="0ce73-142">Aprenda más sobre la creación de web API en [crear su primer Web API con MVC de ASP.NET Core y Visual Studio](../tutorials/first-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="0ce73-142">Learn more about creating web APIs in [Building Your First Web API with ASP.NET Core MVC and Visual Studio](../tutorials/first-web-api.md).</span></span>

## <a name="creating-the-controller"></a><span data-ttu-id="0ce73-143">Crear el controlador</span><span class="sxs-lookup"><span data-stu-id="0ce73-143">Creating the Controller</span></span>

<span data-ttu-id="0ce73-144">Agrega un nuevo controlador para el proyecto *ToDoItemsController*.</span><span class="sxs-lookup"><span data-stu-id="0ce73-144">Add a new controller to the project, *ToDoItemsController*.</span></span> <span data-ttu-id="0ce73-145">Debe heredar de Microsoft.AspNetCore.Mvc.Controller.</span><span class="sxs-lookup"><span data-stu-id="0ce73-145">It should inherit from Microsoft.AspNetCore.Mvc.Controller.</span></span> <span data-ttu-id="0ce73-146">Agregar un `Route` atributo para indicar que el controlador controle las solicitudes realizadas a las rutas de acceso a partir de `api/todoitems`.</span><span class="sxs-lookup"><span data-stu-id="0ce73-146">Add a `Route` attribute to indicate that the controller will handle requests made to paths starting with `api/todoitems`.</span></span> <span data-ttu-id="0ce73-147">El `[controller]` símbolo (token) de la ruta se sustituye por el nombre del controlador (si se omite la `Controller` sufijo) y es especialmente útil para las rutas globales.</span><span class="sxs-lookup"><span data-stu-id="0ce73-147">The `[controller]` token in the route is replaced by the name of the controller (omitting the `Controller` suffix), and is especially helpful for global routes.</span></span> <span data-ttu-id="0ce73-148">Obtenga más información sobre [enrutamiento](../fundamentals/routing.md).</span><span class="sxs-lookup"><span data-stu-id="0ce73-148">Learn more about [routing](../fundamentals/routing.md).</span></span>

<span data-ttu-id="0ce73-149">El controlador necesita una `IToDoRepository` a función; solicitar una instancia de este tipo a través del constructor del controlador.</span><span class="sxs-lookup"><span data-stu-id="0ce73-149">The controller requires an `IToDoRepository` to function; request an instance of this type through the controller's constructor.</span></span> <span data-ttu-id="0ce73-150">En tiempo de ejecución, esta instancia se proporcionarán con el soporte técnico del marco de trabajo para [inyección de dependencia](../fundamentals/dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="0ce73-150">At runtime, this instance will be provided using the framework's support for [dependency injection](../fundamentals/dependency-injection.md).</span></span>

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=1-17&highlight=9,14)]

<span data-ttu-id="0ce73-151">Esta API es compatible con cuatro verbos HTTP diferentes para realizar operaciones CRUD (creación, lectura, actualización, eliminación) en el origen de datos.</span><span class="sxs-lookup"><span data-stu-id="0ce73-151">This API supports four different HTTP verbs to perform CRUD (Create, Read, Update, Delete) operations on the data source.</span></span> <span data-ttu-id="0ce73-152">La más simple de ellas es la operación de lectura, que corresponde a una solicitud HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="0ce73-152">The simplest of these is the Read operation, which corresponds to an HTTP GET request.</span></span>

### <a name="reading-items"></a><span data-ttu-id="0ce73-153">Leer elementos</span><span class="sxs-lookup"><span data-stu-id="0ce73-153">Reading Items</span></span>

<span data-ttu-id="0ce73-154">Solicitar una lista de elementos se realiza con una solicitud GET a la `List` método.</span><span class="sxs-lookup"><span data-stu-id="0ce73-154">Requesting a list of items is done with a GET request to the `List` method.</span></span> <span data-ttu-id="0ce73-155">El `[HttpGet]` del atributo en el `List` método indica que esta acción solo debe controlar las solicitudes GET.</span><span class="sxs-lookup"><span data-stu-id="0ce73-155">The `[HttpGet]` attribute on the `List` method indicates that this action should only handle GET requests.</span></span> <span data-ttu-id="0ce73-156">La ruta de esta acción es la ruta especificada en el controlador.</span><span class="sxs-lookup"><span data-stu-id="0ce73-156">The route for this action is the route specified on the controller.</span></span> <span data-ttu-id="0ce73-157">No es necesario utilizar el nombre de acción como parte de la ruta.</span><span class="sxs-lookup"><span data-stu-id="0ce73-157">You don't necessarily need to use the action name as part of the route.</span></span> <span data-ttu-id="0ce73-158">Solo debe asegurarse de que cada acción tiene una ruta única e inequívoca.</span><span class="sxs-lookup"><span data-stu-id="0ce73-158">You just need to ensure each action has a unique and unambiguous route.</span></span> <span data-ttu-id="0ce73-159">Enrutamiento de atributos se puede aplicar en el controlador y los niveles de método para crear rutas específicas.</span><span class="sxs-lookup"><span data-stu-id="0ce73-159">Routing attributes can be applied at both the controller and method levels to build up specific routes.</span></span>

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=19-23)]

<span data-ttu-id="0ce73-160">El `List` método devuelve un código de 200 respuesta OK y todos los elementos de lista de tareas, serializados como JSON.</span><span class="sxs-lookup"><span data-stu-id="0ce73-160">The `List` method returns a 200 OK response code and all of the ToDo items, serialized as JSON.</span></span>

<span data-ttu-id="0ce73-161">Puede probar el nuevo método de API con una variedad de herramientas, como [Postman](https://www.getpostman.com/docs/), se muestra aquí:</span><span class="sxs-lookup"><span data-stu-id="0ce73-161">You can test your new API method using a variety of tools, such as [Postman](https://www.getpostman.com/docs/), shown here:</span></span>

![Consola de postman que muestra una solicitud GET para todoitems y el cuerpo de la respuesta que muestra el JSON de los tres elementos devueltos](native-mobile-backend/_static/postman-get.png)

### <a name="creating-items"></a><span data-ttu-id="0ce73-163">Creación de elementos</span><span class="sxs-lookup"><span data-stu-id="0ce73-163">Creating Items</span></span>

<span data-ttu-id="0ce73-164">Por convención, la creación de nuevos elementos de datos se asigna para el verbo HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="0ce73-164">By convention, creating new data items is mapped to the HTTP POST verb.</span></span> <span data-ttu-id="0ce73-165">El `Create` método tiene un `[HttpPost]` atributo aplicado y acepta un `ToDoItem` instancia.</span><span class="sxs-lookup"><span data-stu-id="0ce73-165">The `Create` method has an `[HttpPost]` attribute applied to it, and accepts a `ToDoItem` instance.</span></span> <span data-ttu-id="0ce73-166">Puesto que la `item` argumento se pasará en el cuerpo de la publicación, este parámetro se decora con el `[FromBody]` atributo.</span><span class="sxs-lookup"><span data-stu-id="0ce73-166">Since the `item` argument will be passed in the body of the POST, this parameter is decorated with the `[FromBody]` attribute.</span></span>

<span data-ttu-id="0ce73-167">Dentro del método, el elemento se comprueba para validez y existencia anterior en el almacén de datos y, si se producen sin problemas, se agrega con el repositorio.</span><span class="sxs-lookup"><span data-stu-id="0ce73-167">Inside the method, the item is checked for validity and prior existence in the data store, and if no issues occur, it is added using the repository.</span></span> <span data-ttu-id="0ce73-168">Comprobación de `ModelState.IsValid` realiza [validación de modelos](../mvc/models/validation.md)y debe realizarse en cada método de API que acepta datos proporcionados por usuario.</span><span class="sxs-lookup"><span data-stu-id="0ce73-168">Checking `ModelState.IsValid` performs [model validation](../mvc/models/validation.md), and should be done in every API method that accepts user input.</span></span>

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=25-46)]

<span data-ttu-id="0ce73-169">El ejemplo usa una enumeración que contiene códigos de error que se pasan al cliente móvil:</span><span class="sxs-lookup"><span data-stu-id="0ce73-169">The sample uses an enum containing error codes that are passed to the mobile client:</span></span>

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=91-99)]

<span data-ttu-id="0ce73-170">Pruebe a agregar nuevos elementos con Postman eligiendo el verbo POST que proporciona el nuevo objeto en formato JSON en el cuerpo de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="0ce73-170">Test adding new items using Postman by choosing the POST verb providing the new object in JSON format in the Body of the request.</span></span> <span data-ttu-id="0ce73-171">También debe agregar un encabezado de solicitud que especifica un `Content-Type` de `application/json`.</span><span class="sxs-lookup"><span data-stu-id="0ce73-171">You should also add a request header specifying a `Content-Type` of `application/json`.</span></span>

![Consola postman que muestra una entrada y una respuesta](native-mobile-backend/_static/postman-post.png)

<span data-ttu-id="0ce73-173">El método devuelve el elemento recién creado en la respuesta.</span><span class="sxs-lookup"><span data-stu-id="0ce73-173">The method returns the newly created item in the response.</span></span>

### <a name="updating-items"></a><span data-ttu-id="0ce73-174">Actualizar elementos</span><span class="sxs-lookup"><span data-stu-id="0ce73-174">Updating Items</span></span>

<span data-ttu-id="0ce73-175">Modificar registros se realiza mediante solicitudes HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="0ce73-175">Modifying records is done using HTTP PUT requests.</span></span> <span data-ttu-id="0ce73-176">Aparte de este cambio, el `Edit` método es casi idéntico a `Create`.</span><span class="sxs-lookup"><span data-stu-id="0ce73-176">Other than this change, the `Edit` method is almost identical to `Create`.</span></span> <span data-ttu-id="0ce73-177">Tenga en cuenta que si no se encuentra el registro, el `Edit` acción devolverá un `NotFound` respuesta (404).</span><span class="sxs-lookup"><span data-stu-id="0ce73-177">Note that if the record isn't found, the `Edit` action will return a `NotFound` (404) response.</span></span>

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=48-69)]

<span data-ttu-id="0ce73-178">Para probar con Postman, cambie el verbo PUT.</span><span class="sxs-lookup"><span data-stu-id="0ce73-178">To test with Postman, change the verb to PUT.</span></span> <span data-ttu-id="0ce73-179">Especifique los datos actualizados del objeto en el cuerpo de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="0ce73-179">Specify the updated object data in the Body of the request.</span></span>

![Consola postman que muestra una respuesta y PUT](native-mobile-backend/_static/postman-put.png)

<span data-ttu-id="0ce73-181">Este método devuelve un `NoContent` respuesta (204) cuando se realiza correctamente, para mantener la coherencia con la API existente.</span><span class="sxs-lookup"><span data-stu-id="0ce73-181">This method returns a `NoContent` (204) response when successful, for consistency with the pre-existing API.</span></span>

### <a name="deleting-items"></a><span data-ttu-id="0ce73-182">Eliminar elementos</span><span class="sxs-lookup"><span data-stu-id="0ce73-182">Deleting Items</span></span>

<span data-ttu-id="0ce73-183">Eliminación de registros se consigue realizar las solicitudes de eliminación en el servicio y pasar el identificador del elemento que va a eliminar.</span><span class="sxs-lookup"><span data-stu-id="0ce73-183">Deleting records is accomplished by making DELETE requests to the service, and passing the ID of the item to be deleted.</span></span> <span data-ttu-id="0ce73-184">Como con las actualizaciones, se recibirán las solicitudes para los elementos que no existen `NotFound` las respuestas.</span><span class="sxs-lookup"><span data-stu-id="0ce73-184">As with updates, requests for items that don't exist will receive `NotFound` responses.</span></span> <span data-ttu-id="0ce73-185">De lo contrario, obtendrá una solicitud correcta un `NoContent` respuesta (204).</span><span class="sxs-lookup"><span data-stu-id="0ce73-185">Otherwise, a successful request will get a `NoContent` (204) response.</span></span>

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=71-88)]

<span data-ttu-id="0ce73-186">Tenga en cuenta que al probar la funcionalidad de eliminar, nada en el cuerpo de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="0ce73-186">Note that when testing the delete functionality, nothing is required in the Body of the request.</span></span>

![Consola de postman con una eliminación y una respuesta](native-mobile-backend/_static/postman-delete.png)

## <a name="common-web-api-conventions"></a><span data-ttu-id="0ce73-188">Convenciones de Web API comunes</span><span class="sxs-lookup"><span data-stu-id="0ce73-188">Common Web API Conventions</span></span>

<span data-ttu-id="0ce73-189">Al desarrollar los servicios back-end de la aplicación, desea tener acceso a un conjunto coherente de directivas para controlar cuestiones de corte del cruce o convenciones.</span><span class="sxs-lookup"><span data-stu-id="0ce73-189">As you develop the backend services for your app, you will want to come up with a consistent set of conventions or policies for handling cross-cutting concerns.</span></span> <span data-ttu-id="0ce73-190">Por ejemplo, en el servicio mostrado anteriormente, las solicitudes para los registros específicos que no se encontraron recibirlos un `NotFound` respuesta, en lugar de un `BadRequest` respuesta.</span><span class="sxs-lookup"><span data-stu-id="0ce73-190">For example, in the service shown above, requests for specific records that weren't found received a `NotFound` response, rather than a `BadRequest` response.</span></span> <span data-ttu-id="0ce73-191">De forma similar, los comandos realizados para este servicio que pasa en tipos enlazada a un modelo siempre comprueba `ModelState.IsValid` y devuelve un `BadRequest` para los tipos de modelo no válido.</span><span class="sxs-lookup"><span data-stu-id="0ce73-191">Similarly, commands made to this service that passed in model bound types always checked `ModelState.IsValid` and returned a `BadRequest` for invalid model types.</span></span>

<span data-ttu-id="0ce73-192">Después de identificar una directiva común para las API, normalmente puede encapsular en un [filtro](../mvc/controllers/filters.md).</span><span class="sxs-lookup"><span data-stu-id="0ce73-192">Once you've identified a common policy for your APIs, you can usually encapsulate it in a [filter](../mvc/controllers/filters.md).</span></span> <span data-ttu-id="0ce73-193">Obtenga más información sobre [cómo encapsular directivas API comunes en aplicaciones de MVC de ASP.NET Core](https://msdn.microsoft.com/magazine/mt767699.aspx).</span><span class="sxs-lookup"><span data-stu-id="0ce73-193">Learn more about [how to encapsulate common API policies in ASP.NET Core MVC applications](https://msdn.microsoft.com/magazine/mt767699.aspx).</span></span>
