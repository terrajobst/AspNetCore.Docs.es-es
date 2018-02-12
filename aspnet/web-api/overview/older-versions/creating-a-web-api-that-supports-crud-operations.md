---
uid: web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
title: Habilitar las operaciones CRUD en ASP.NET Web API 1 | Documentos de Microsoft
author: MikeWasson
description: "Este tutorial muestra cómo admitir operaciones CRUD en un servicio HTTP mediante ASP.NET Web API. Versiones de software que se usa en el tutorial Web PA de Visual Studio 2012..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/28/2012
ms.topic: article
ms.assetid: c125ca47-606a-4d6f-a1fc-1fc62928af93
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
msc.type: authoredcontent
ms.openlocfilehash: 69b7d5453b6ff36d6e28a69428b016cb8cfd06e9
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/12/2018
---
<a name="enabling-crud-operations-in-aspnet-web-api-1"></a>Habilitar las operaciones CRUD en ASP.NET Web API 1
====================
por [Mike Wasson](https://github.com/MikeWasson)

[Descargar el proyecto completado](http://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-c4761894)

> Este tutorial muestra cómo admitir operaciones CRUD en un servicio HTTP mediante ASP.NET Web API.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software que se usa en el tutorial
> 
> 
> - Visual Studio 2012
> - API Web 1 (también funciona con la API Web 2)


Es el acrónimo CRUD &quot;creación, lectura, actualización y eliminación,&quot; ¿cuáles son las cuatro operaciones de base de datos básica. Muchos servicios HTTP también modelar las operaciones CRUD a través de REST o API de REST.

En este tutorial, compilará una API para administrar una lista de productos de web muy sencilla. Cada producto contendrá un nombre, el precio y la categoría (como &quot;toys&quot; o &quot;hardware&quot;), además de un identificador de producto.

Los productos de API expondrá siguientes métodos.

| Acción | Método HTTP | URI relativo |
| --- | --- | --- |
| Obtener una lista de todos los productos | GET | productos/api / |
| Obtener un producto por Id. | GET | /api/products/*id* |
| Obtener un producto por categoría | GET | /api/products?category=*category* |
| Crear un nuevo producto | EXPONER | productos/api / |
| Actualizar un producto | PUT | /api/products/*id* |
| Eliminar un producto | SUPRIMIR | /api/products/*id* |

Ten en cuenta que algunos de los URI incluyen el identificador de producto en la ruta de acceso. Por ejemplo, para obtener el producto cuyo identificador es 28, el cliente envía una solicitud GET `http://hostname/api/products/28`.

### <a name="resources"></a>Recursos

Los productos de API define el URI para dos tipos de recursos:

| Recurso | Identificador URI |
| --- | --- |
| La lista de todos los productos. | productos/api / |
| Un producto individual. | /api/products/*id* |

### <a name="methods"></a>Métodos

Los cuatro principales métodos HTTP (GET, PUT, POST y DELETE) se pueden asignar a las operaciones CRUD como sigue:

- GET recupera la representación del recurso en un URI especificado. GET no debe tener efectos deseados en el servidor.
- PUT actualiza un recurso en un URI especificado. PUT también puede utilizarse para crear un nuevo recurso en un URI especificado, si el servidor permite a los clientes especificar a URI nuevo. Para este tutorial, la API no admitirá la creación a través de PUT.
- POST crea un nuevo recurso. El servidor asigna el URI para el nuevo objeto y devuelve este URI como parte del mensaje de respuesta.
- El método DELETE Elimina un recurso en un URI especificado.

Nota: El método PUT reemplaza la entidad completa de productos. Es decir, el cliente debe enviar una representación completa del producto actualizada. Si desea admitir actualizaciones parciales, es preferible el método de revisión. Este tutorial no implementa la revisión.

## <a name="create-a-new-web-api-project"></a>Crear un nuevo proyecto de API de Web

Comience ejecutando Visual Studio y seleccione **nuevo proyecto** desde el **iniciar** página. O bien, en la **archivo** menú, seleccione **New** y, a continuación, **proyecto**.

En el **plantillas** panel, seleccione **plantillas instaladas** y expanda el **Visual C#** nodo. En **Visual C#**, seleccione **Web**. En la lista de plantillas de proyecto, seleccione **aplicación Web de ASP.NET MVC 4**. Denomine el proyecto &quot;ProductStore&quot; y haga clic en **Aceptar**.

![](creating-a-web-api-that-supports-crud-operations/_static/image1.png)

En el **nuevo proyecto de ASP.NET MVC 4** cuadro de diálogo, seleccione **API Web** y haga clic en **Aceptar**.

![](creating-a-web-api-that-supports-crud-operations/_static/image2.png)

## <a name="adding-a-model"></a>Agregar un modelo

Un *modelo* es un objeto que representa los datos de la aplicación. En ASP.NET Web API, puede usar los objetos CLR fuertemente tipados como modelos y automáticamente se serializará a XML o JSON para el cliente.

Para la API ProductStore, nuestros datos consisten de los productos, por lo que vamos a crear una nueva clase denominada `Product`.

Si el Explorador de soluciones no está visible, haga clic en el **vista** menú y seleccione **el Explorador de soluciones**. En el Explorador de soluciones, haga clic en el **modelos** carpeta. En el contexto meny, seleccione **agregar**, a continuación, seleccione **clase**. Nombre de la clase &quot;producto&quot;.

![](creating-a-web-api-that-supports-crud-operations/_static/image3.png)

Agregue las siguientes propiedades para el `Product` clase.

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample1.cs)]

## <a name="adding-a-repository"></a>Agregar un repositorio

Es necesario almacenar una colección de productos. Es una buena idea para separar la colección de la implementación del servicio. De este modo, podemos cambiar la memoria auxiliar sin volver a escribir la clase de servicio. Este tipo de diseño se denomina la *repositorio* patrón. Empiece por definir una interfaz genérica para el repositorio.

En el Explorador de soluciones, haga clic en el **modelos** carpeta. Seleccione **agregar**, a continuación, seleccione **nuevo elemento**.

![](creating-a-web-api-that-supports-crud-operations/_static/image4.png)

En el **plantillas** panel, seleccione **plantillas instaladas** y expanda el nodo C#. En C#, seleccione **código**. En la lista de plantillas de código, seleccione **interfaz**. Nombre de la interfaz &quot;IProductRepository&quot;.

![](creating-a-web-api-that-supports-crud-operations/_static/image5.png)

Agregue la siguiente implementación:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample2.cs)]

Ahora agregue otra clase en la carpeta Models, denominada &quot;ProductRepository.&quot; Esta clase implementará la interfaz `IProductRespository`. Agregue la siguiente implementación:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample3.cs)]

El repositorio mantiene la lista en la memoria local. Esto es correcto para obtener un tutorial, pero en una aplicación real, podría almacenar los datos externamente, una base de datos o en el almacenamiento en la nube. El modelo de repositorio le resultará más fácil cambiar la implementación más adelante.

## <a name="adding-a-web-api-controller"></a>Cómo agregar un controlador de API Web

Si ha trabajado con ASP.NET MVC, a continuación, ya están familiarizados con los controladores. En ASP.NET Web API, un *controlador* es una clase que controla las solicitudes HTTP desde el cliente. El Asistente para nuevo proyecto crea dos controladores cuando creó el proyecto. Para verlas, expanda la carpeta de controladores en el Explorador de soluciones.

- HomeController es un controlador de MVC de ASP.NET tradicional. Es responsable de servir páginas HTML para el sitio y no está directamente relacionado con la API web.
- ValuesController es un controlador de WebAPI de ejemplo.

Voy a eliminar ValuesController, haciendo clic en el archivo en el Explorador de soluciones y seleccionando **eliminar.** Ahora agregue un nuevo controlador, como se indica a continuación:

En **el Explorador de soluciones**, haga clic en la carpeta de controladores. Seleccione **agregar** y, a continuación, seleccione **controlador**.

![](creating-a-web-api-that-supports-crud-operations/_static/image6.png)

En el **Agregar controlador** asistente, el nombre del controlador &quot;ProductsController&quot;. En el **plantilla** lista desplegable, seleccione **controlador de API en blanco**. A continuación, haga clic en **Agregar**.

![](creating-a-web-api-that-supports-crud-operations/_static/image7.png)

> [!NOTE]
> No es necesario poner los controladores en una carpeta denominada controladores. El nombre de carpeta no es importante; es simplemente una manera cómoda de organizar los archivos de origen.


El **Agregar controlador** asistente creará un archivo denominado ProductsController.cs en la carpeta de controladores. Si este archivo ya no está abierto, haga doble clic en el archivo para abrirlo. Agregue el siguiente **con** instrucción:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample4.cs)]

Agregar un campo que contiene un **IProductRepository** instancia.

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample5.cs)]

> [!NOTE]
> Al llamar a `new ProductRepository()` en el controlador no es el mejor diseño, porque asocia el controlador a una implementación concreta de `IProductRepository`. Para obtener un mejor enfoque, consulte [mediante la resolución de dependencia de la API de Web](../advanced/dependency-injection.md).


## <a name="getting-a-resource"></a>Obtener un recurso

La API ProductStore expondrá varios &quot;leer&quot; acciones como métodos GET de HTTP. Cada acción se corresponderá a un método en el `ProductsController` clase.

| Acción | Método HTTP | URI relativo |
| --- | --- | --- |
| Obtener una lista de todos los productos | GET | productos/api / |
| Obtener un producto por Id. | GET | /api/products/*id* |
| Obtener un producto por categoría | GET | /api/products?category=*category* |

Para obtener la lista de todos los productos, agregue este método a la `ProductsController` clase:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample6.cs)]

El nombre del método empieza con &quot;obtener&quot;, por lo que por convención, se asigna a las solicitudes GET. Además, dado que el método no tiene parámetros, se asigna a un URI que no contenga un  *&quot;identificador&quot;*  segmento en la ruta de acceso.

Para obtener un producto por el identificador, agregue este método para el `ProductsController` clase:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample7.cs)]

Este nombre de método también se inicia con &quot;obtener&quot;, pero el método tiene un parámetro denominado *identificador*. Este parámetro se asigna a la &quot;identificador&quot; segmento de la ruta de acceso URI. El marco de trabajo de ASP.NET Web API convierte automáticamente el identificador para el tipo de datos correcto (**int**) para el parámetro.

El método GetProduct produce una excepción de tipo **HttpResponseException** si *identificador* no es válido. Esta excepción se traducirá por el marco de trabajo en un error 404 (no encontrado).

Por último, agregue un método para buscar productos por categoría:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample8.cs)]

Si el URI de solicitud tiene una cadena de consulta, API Web intenta buscar coincidencias con los parámetros de consulta en los parámetros en el método del controlador. Por lo tanto, un URI del formulario "api/products? categoría =*categoría*" se asignará a este método.

## <a name="creating-a-resource"></a>Crear un recurso

A continuación, vamos a agregar un método a la `ProductsController` clase para crear un nuevo producto. Esta es una implementación sencilla del método:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample9.cs)]

Tenga en cuenta dos cosas acerca de este método:

- El nombre del método empieza con &quot;Post... &quot;. Para crear un nuevo producto, el cliente envía una solicitud HTTP POST.
- El método toma un parámetro de tipo Product. En la API de Web, parámetros con tipos complejos se deserializan el cuerpo de solicitud. Por lo tanto, esperamos que el cliente envíe una representación serializada de un objeto de producto, en formato XML o JSON.

Esta implementación funcionará, pero no es muy completa. Idealmente, nos gustaría que la respuesta HTTP para incluir lo siguiente:

- **Código de respuesta:** de forma predeterminada, el marco Web API establece el código de estado de respuesta en 200 (OK). Pero, según el protocolo HTTP/1.1, cuando una solicitud POST da como resultado la creación de un recurso, el servidor debe responder con el estado 201 (creado).
- **Ubicación:** cuando el servidor crea un recurso, debe incluir el URI del recurso nuevo en el encabezado Location de la respuesta.

ASP.NET Web API facilita la manipular el mensaje de respuesta HTTP. Aquí se muestra la implementación mejorada:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample10.cs)]

Observe que el tipo de valor devuelto del método es ahora **HttpResponseMessage**. Devolviendo un **HttpResponseMessage** en lugar de un producto, podemos controlar los detalles del mensaje de respuesta HTTP, incluido el código de estado y el encabezado de ubicación.

El **CreateResponse** método crea un **HttpResponseMessage** y automáticamente se escribe una representación serializada del objeto Product en el cuerpo fo el mensaje de respuesta.

> [!NOTE]
> En este ejemplo no valida el `Product`. Para obtener información acerca de la validación del modelo, vea [validación del modelo en ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).


## <a name="updating-a-resource"></a>Actualización de un recurso

Actualizar un producto con PUT es sencillo:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample11.cs)]

El nombre del método empieza con &quot;colocar... &quot;, por lo que la API Web hace coincidir con las solicitudes PUT. El método toma dos parámetros, el identificador de producto y el producto actualizado. El *identificador* parámetro procede de la ruta de acceso URI y el *producto* parámetro se deserializa el cuerpo de solicitud. De forma predeterminada, el marco de trabajo de ASP.NET Web API tiene tipos de parámetro simples desde la ruta de acceso y tipos complejos en el cuerpo de solicitud.

## <a name="deleting-a-resource"></a>Si se elimina un recurso

Para eliminar un recurso, defina un método de "Eliminar...".

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample12.cs)]

Si una solicitud de eliminación se realiza correctamente, puede devolver el estado 200 (OK) con un cuerpo de entidad que describe el estado; estado 202 (aceptado) si la eliminación sigue siendo pendiente; o estado 204 (sin contenido) con ningún cuerpo de la entidad. En este caso, el `DeleteProduct` método tiene un `void` tipo de valor devuelto, por lo que ASP.NET Web API lo convierte automáticamente en el estado de código 204 (sin contenido).
