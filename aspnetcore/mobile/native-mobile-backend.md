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
ms.openlocfilehash: 2edb704ea6875e8aa70e79fe085cc0edbe0c9a55
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
# <a name="creating-backend-services-for-native-mobile-applications"></a>Creación de servicios back-end para aplicaciones móviles nativas

Por [Steve Smith](https://ardalis.com/)

Aplicaciones móviles pueden comunicarse fácilmente con servicios back-end de ASP.NET Core.

[Ver o descargar código servicios back-end](https://github.com/aspnet/Docs/tree/master/aspnetcore/mobile/native-mobile-backend/sample)

## <a name="the-sample-native-mobile-app"></a>El ejemplo de aplicación nativa de móvil

Este tutorial muestra cómo crear servicios back-end mediante ASP.NET Core MVC para admitir aplicaciones móviles nativas. Usa el [aplicación Xamarin Forms ToDoRest](https://developer.xamarin.com/guides/xamarin-forms/web-services/consuming/rest/) como su cliente nativo, lo que incluye clientes nativos independientes para dispositivos Android, iOS, universales de Windows y Windows Phone. Puede seguir el tutorial vinculado para crear la aplicación nativa (e instalar las herramientas de Xamarin libres necesarias), así como descargar la solución de ejemplo de Xamarin. El ejemplo de Xamarin incluye un proyecto de servicios de ASP.NET Web API 2, que sustituye a aplicaciones de ASP.NET Core de este artículo (con sin cambios requeridos por el cliente).

![Para la aplicación realice Rest que se ejecuta en un smartphone Android](native-mobile-backend/_static/todo-android.png)

### <a name="features"></a>Características

Es compatible con la aplicación ToDoRest enumerar, agregar, eliminar y actualizar elementos pendientes. Cada elemento tiene un identificador, un nombre, notas y una propiedad que indica si se ha realizado todavía.

La vista principal de los elementos, como se muestra arriba, muestra el nombre de cada elemento e indica si se realiza con una marca de verificación.

Al puntear en el `+` icono abre un cuadro de diálogo Agregar elemento:

![Agregar cuadro de diálogo elemento](native-mobile-backend/_static/todo-android-new-item.png)

Al puntear en un elemento en la pantalla principal de lista, se abre un cuadro de diálogo Editar, donde se puede modificar el nombre del elemento, notas y configuración listo, o se puede eliminar el elemento:

![Editar cuadro de diálogo elemento](native-mobile-backend/_static/todo-android-edit-item.png)

En este ejemplo se configura de forma predeterminada para utilizar servicios de back-end alojados en developer.xamarin.com, lo que permite operaciones de solo lectura. Para probarlo usted mismo con la aplicación de ASP.NET Core que creó en la sección siguiente que se ejecutan en el equipo, debe actualizar la aplicación `RestUrl` constante. Navegue hasta la `ToDoREST` proyecto y abra el *Constants.cs* archivo. Reemplace la `RestUrl` con una dirección URL que incluye IP su equipo dirección (no localhost ni 127.0.0.1, puesto que esta dirección se utiliza desde el emulador de dispositivo, no desde el equipo). Incluya también el número de puerto (5000). Para comprobar que los servicios funcionan con un dispositivo, asegúrese de que no tiene un active firewall bloqueando el acceso a este puerto.

```csharp
// URL of REST service (Xamarin ReadOnly Service)
//public static string RestUrl = "http://developer.xamarin.com:8081/api/todoitems{0}";

// use your machine's IP address
public static string RestUrl = "http://192.168.1.207:5000/api/todoitems/{0}";
```

## <a name="creating-the-aspnet-core-project"></a>Crear el proyecto de ASP.NET Core

Crear una nueva aplicación Web de ASP.NET Core en Visual Studio. Elija la plantilla de API Web y sin autenticación. Denomine el proyecto *ToDoApi*.

![Cuadro de diálogo nueva aplicación Web ASP.NET con plantilla de proyecto de API Web seleccionado](native-mobile-backend/_static/web-api-template.png)

La aplicación debe responder a todas las solicitudes realizadas al puerto 5000. Actualización *Program.cs* para incluir `.UseUrls("http://*:5000")` para lograr esto:

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Program.cs?range=10-16&highlight=3)]

> [!NOTE]
> Asegúrese de que se ejecute la aplicación directamente, en lugar de detrás de IIS Express, que omite las solicitudes no locales de forma predeterminada. Ejecute `dotnet run` desde un símbolo del sistema, o elija el perfil de nombre de aplicación en la lista desplegable de destino de depuración en la barra de herramientas de Visual Studio.

Agregar una clase de modelo para representar elementos pendientes. Marca requiere campos mediante el `[Required]` atributo:

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Models/ToDoItem.cs)]

Los métodos de API requieren una manera de trabajar con datos. Use el mismo `IToDoRepository` interfaz los usos de ejemplo originales de Xamarin:

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Interfaces/IToDoRepository.cs)]

En este ejemplo, la implementación usa solo una colección de elementos privada:

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Services/ToDoRepository.cs)]

Configurar la implementación en *Startup.cs*:

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Startup.cs?highlight=6&range=29-35)]

En este punto, está listo para crear el *ToDoItemsController*.

> [!TIP]
> Aprenda más sobre la creación de web API en [crear su primer Web API con MVC de ASP.NET Core y Visual Studio](../tutorials/first-web-api.md).

## <a name="creating-the-controller"></a>Crear el controlador

Agrega un nuevo controlador para el proyecto *ToDoItemsController*. Debe heredar de Microsoft.AspNetCore.Mvc.Controller. Agregar un `Route` atributo para indicar que el controlador controle las solicitudes realizadas a las rutas de acceso a partir de `api/todoitems`. El `[controller]` símbolo (token) de la ruta se sustituye por el nombre del controlador (si se omite la `Controller` sufijo) y es especialmente útil para las rutas globales. Obtenga más información sobre [enrutamiento](../fundamentals/routing.md).

El controlador necesita una `IToDoRepository` a función; solicitar una instancia de este tipo a través del constructor del controlador. En tiempo de ejecución, esta instancia se proporcionarán con el soporte técnico del marco de trabajo para [inyección de dependencia](../fundamentals/dependency-injection.md).

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=1-17&highlight=9,14)]

Esta API es compatible con cuatro verbos HTTP diferentes para realizar operaciones CRUD (creación, lectura, actualización, eliminación) en el origen de datos. La más simple de ellas es la operación de lectura, que corresponde a una solicitud HTTP GET.

### <a name="reading-items"></a>Leer elementos

Solicitar una lista de elementos se realiza con una solicitud GET a la `List` método. El `[HttpGet]` del atributo en el `List` método indica que esta acción solo debe controlar las solicitudes GET. La ruta de esta acción es la ruta especificada en el controlador. No es necesario utilizar el nombre de acción como parte de la ruta. Solo debe asegurarse de que cada acción tiene una ruta única e inequívoca. Enrutamiento de atributos se puede aplicar en el controlador y los niveles de método para crear rutas específicas.

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=19-23)]

El `List` método devuelve un código de 200 respuesta OK y todos los elementos de lista de tareas, serializados como JSON.

Puede probar el nuevo método de API con una variedad de herramientas, como [Postman](https://www.getpostman.com/docs/), se muestra aquí:

![Consola de postman que muestra una solicitud GET para todoitems y el cuerpo de la respuesta que muestra el JSON de los tres elementos devueltos](native-mobile-backend/_static/postman-get.png)

### <a name="creating-items"></a>Creación de elementos

Por convención, la creación de nuevos elementos de datos se asigna para el verbo HTTP POST. El `Create` método tiene un `[HttpPost]` atributo aplicado y acepta un `ToDoItem` instancia. Puesto que la `item` argumento se pasará en el cuerpo de la publicación, este parámetro se decora con el `[FromBody]` atributo.

Dentro del método, el elemento se comprueba para validez y existencia anterior en el almacén de datos y, si se producen sin problemas, se agrega con el repositorio. Comprobación de `ModelState.IsValid` realiza [validación de modelos](../mvc/models/validation.md)y debe realizarse en cada método de API que acepta datos proporcionados por usuario.

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=25-46)]

El ejemplo usa una enumeración que contiene códigos de error que se pasan al cliente móvil:

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=91-99)]

Pruebe a agregar nuevos elementos con Postman eligiendo el verbo POST que proporciona el nuevo objeto en formato JSON en el cuerpo de la solicitud. También debe agregar un encabezado de solicitud que especifica un `Content-Type` de `application/json`.

![Consola postman que muestra una entrada y una respuesta](native-mobile-backend/_static/postman-post.png)

El método devuelve el elemento recién creado en la respuesta.

### <a name="updating-items"></a>Actualizar elementos

Modificar registros se realiza mediante solicitudes HTTP PUT. Aparte de este cambio, el `Edit` método es casi idéntico a `Create`. Tenga en cuenta que si no se encuentra el registro, el `Edit` acción devolverá un `NotFound` respuesta (404).

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=48-69)]

Para probar con Postman, cambie el verbo PUT. Especifique los datos actualizados del objeto en el cuerpo de la solicitud.

![Consola postman que muestra una respuesta y PUT](native-mobile-backend/_static/postman-put.png)

Este método devuelve un `NoContent` respuesta (204) cuando se realiza correctamente, para mantener la coherencia con la API existente.

### <a name="deleting-items"></a>Eliminar elementos

Eliminación de registros se consigue realizar las solicitudes de eliminación en el servicio y pasar el identificador del elemento que va a eliminar. Como con las actualizaciones, se recibirán las solicitudes para los elementos que no existen `NotFound` las respuestas. De lo contrario, obtendrá una solicitud correcta un `NoContent` respuesta (204).

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=71-88)]

Tenga en cuenta que al probar la funcionalidad de eliminar, nada en el cuerpo de la solicitud.

![Consola de postman con una eliminación y una respuesta](native-mobile-backend/_static/postman-delete.png)

## <a name="common-web-api-conventions"></a>Convenciones de Web API comunes

Al desarrollar los servicios back-end de la aplicación, desea tener acceso a un conjunto coherente de directivas para controlar cuestiones de corte del cruce o convenciones. Por ejemplo, en el servicio mostrado anteriormente, las solicitudes para los registros específicos que no se encontraron recibirlos un `NotFound` respuesta, en lugar de un `BadRequest` respuesta. De forma similar, los comandos realizados para este servicio que pasa en tipos enlazada a un modelo siempre comprueba `ModelState.IsValid` y devuelve un `BadRequest` para los tipos de modelo no válido.

Después de identificar una directiva común para las API, normalmente puede encapsular en un [filtro](../mvc/controllers/filters.md). Obtenga más información sobre [cómo encapsular directivas API comunes en aplicaciones de MVC de ASP.NET Core](https://msdn.microsoft.com/magazine/mt767699.aspx).
