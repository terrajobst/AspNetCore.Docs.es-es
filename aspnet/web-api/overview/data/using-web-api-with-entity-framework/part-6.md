---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-6
title: Crear el cliente de JavaScript | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 06/16/2014
ms.assetid: 20360326-b123-4b1e-abae-1d350edf4ce4
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-6
msc.type: authoredcontent
ms.openlocfilehash: acadd8100b2698b4b17a750a02f875d4fd51c414
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37826522"
---
<a name="create-the-javascript-client"></a>Crear al cliente de JavaScript
====================
por [Mike Wasson](https://github.com/MikeWasson)

[Descargue el proyecto completado](https://github.com/MikeWasson/BookService)

En esta sección, creará el cliente para la aplicación, mediante HTML, JavaScript y el [Knockout.js](http://knockoutjs.com/) biblioteca. Vamos a crear la aplicación cliente en fases:

- Muestra una lista de los libros en pantalla.
- Mostrar detalles de un libro.
- Agregar un nuevo libro.

La biblioteca Knockout usa el patrón Model-View-ViewModel (MVVM):

- El **modelo** es la representación del lado servidor de los datos en el dominio de negocio (en nuestro caso, libros y autores).
- El **vista** es la capa de presentación (HTML).
- El **modelo de vista** es un objeto de JavaScript que contiene los modelos. El modelo de vista es una abstracción de código de la interfaz de usuario. No tiene ningún conocimiento de la representación de HTML. En su lugar, características abstractas de la vista, representa como &quot;una lista de libros&quot;.

La vista está enlazada a datos al modelo de vista. Actualizaciones para el modelo de vista se reflejan automáticamente en la vista. El modelo de vista también obtiene los eventos de la vista, como clics de botón.

![](part-6/_static/image1.png)

Este enfoque facilita cambiar el diseño y la interfaz de usuario de la aplicación, ya que puede cambiar los enlaces, sin tener que reescribir código. Por ejemplo, podría mostrar una lista de elementos como un `<ul>`, cambiarlo más adelante a una tabla.

## <a name="add-the-knockout-library"></a>Agregue la biblioteca Knockout

En Visual Studio, desde el **herramientas** menú, seleccione **Administrador de paquetes de biblioteca**. A continuación, seleccione **Package Manager Console**. En la ventana de consola de administrador de paquetes, escriba el siguiente comando:

[!code-console[Main](part-6/samples/sample1.cmd)]

Este comando agrega los archivos de Knockout en la carpeta de Scripts.

## <a name="create-the-view-model"></a>Crear el modelo de vista

Agregue un archivo JavaScript denominado app.js a la carpeta Scripts. (En el Explorador de soluciones, haga clic en la carpeta de Scripts, seleccione **agregar**, a continuación, seleccione **archivo JavaScript**.) Pegue el código siguiente:

[!code-javascript[Main](part-6/samples/sample2.js)]

En Knockout, la `observable` clase permite el enlace de datos. Cuando se cambia el contenido de un objeto observable, el objeto observable notifica a todos los controles enlazados a datos, por lo que pueden actualizar a sí mismos. (El `observableArray` clase es la versión de la matriz de *observable*.) Para empezar, nuestro modelo de vista tiene dos objetos observables:

- `books` contiene la lista de los libros en pantalla.
- `error` contiene un mensaje de error si se produce un error en una llamada AJAX.

El `getAllBooks` método realiza una llamada AJAX al obtener la lista de los libros en pantalla. A continuación, inserta el resultado en el `books` matriz.

El `ko.applyBindings` método forma parte de la biblioteca Knockout. Toma el modelo de vista como un parámetro y establece el enlace de datos.

## <a name="add-a-script-bundle"></a>Agregar un paquete de scripts

La unión es una característica de ASP.NET 4.5 que facilita combinar o agrupar varios archivos en un único archivo. La unión reduce el número de solicitudes al servidor, lo que puede mejorar el tiempo de carga de página.

Abra el archivo de aplicación\_Start/BundleConfig.cs. Agregue el código siguiente al método RegisterBundles.

[!code-csharp[Main](part-6/samples/sample3.cs)]

> [!div class="step-by-step"]
> [Anterior](part-5.md)
> [Siguiente](part-7.md)
