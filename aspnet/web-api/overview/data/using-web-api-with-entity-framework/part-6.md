---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-6
title: Crear el cliente de JavaScript | Documentos de Microsoft
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 20360326-b123-4b1e-abae-1d350edf4ce4
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-6
msc.type: authoredcontent
ms.openlocfilehash: b397c5a413ae213c9b79da1c0e0626efe21c7e21
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="create-the-javascript-client"></a>Crear al cliente de JavaScript
====================
por [Mike Wasson](https://github.com/MikeWasson)

[Descargar el proyecto completado](https://github.com/MikeWasson/BookService)

En esta sección, aprenderá a crear el cliente para la aplicación, con HTML, JavaScript y el [Knockout.js](http://knockoutjs.com/) biblioteca. Vamos a crear la aplicación cliente en fases:

- Mostrar una lista de libros.
- Mostrar detalles de un libro.
- Agregar un nuevo libro.

La biblioteca de Knockout usa el modelo Model-View-ViewModel (MVVM):

- El **modelo** es la representación del lado del servidor de los datos en el dominio de negocio (en nuestro caso, libros y autores).
- El **vista** es la capa de presentación (HTML).
- El **modelo de vista** es un objeto de JavaScript que contiene los modelos. El modelo de vista es una abstracción de código de la interfaz de usuario. No tiene ningún conocimiento de la representación de HTML. En su lugar, características abstractas de la vista, representa como &quot;una lista de libros&quot;.

La vista está enlazado a datos para el modelo de vista. Actualizaciones para el modelo de vista se reflejan automáticamente en la vista. El modelo de vista también obtiene los eventos de la vista, como clics de botón.

![](part-6/_static/image1.png)

Este enfoque resulta muy sencillo cambiar el diseño y la interfaz de usuario de la aplicación, ya que puede cambiar los enlaces, sin volver a escribir ningún código. Por ejemplo, podría mostrar una lista de elementos como un `<ul>`, cambiarlo más adelante en una tabla.

## <a name="add-the-knockout-library"></a>Agregue la biblioteca de cobertura

En Visual Studio, desde la **herramientas** menú, seleccione **Administrador de paquetes de biblioteca**. A continuación, seleccione **Package Manager Console**. En la ventana de la consola de administrador de paquetes, escriba el siguiente comando:

[!code-console[Main](part-6/samples/sample1.cmd)]

Este comando agrega los archivos de cobertura a la carpeta de Scripts.

## <a name="create-the-view-model"></a>Crear el modelo de vista

Agregue un archivo JavaScript denominado app.js a la carpeta de Scripts. (En el Explorador de soluciones, haga clic en la carpeta Scripts, seleccione **agregar**, a continuación, seleccione **archivo JavaScript**.) Pegue el código siguiente:

[!code-javascript[Main](part-6/samples/sample2.js)]

En Knockout, la `observable` clase habilita el enlace de datos. Cuando se cambia el contenido de un objeto observable, el observable notifica a todos los controles enlazados a datos, para que puedan actualizar por sí mismos. (El `observableArray` clase es la versión de la matriz de *observable*.) Para empezar con nuestro modelo de vista tiene dos objetos observables:

- `books`contiene la lista de libros.
- `error`contiene un mensaje de error si se produce un error en una llamada de AJAX.

El `getAllBooks` método realiza una llamada AJAX para obtener la lista de libros. A continuación, inserta el resultado en la `books` matriz.

El `ko.applyBindings` método forma parte de la biblioteca de cobertura. Toma el modelo de vista como un parámetro y establece el enlace de datos.

## <a name="add-a-script-bundle"></a>Agregar un paquete de scripts

Cómo agrupar es una característica de ASP.NET 4.5 que facilita el proceso combinar o agrupar varios archivos en un único archivo. Cómo agrupar, reduce el número de solicitudes al servidor, lo que puede mejorar el tiempo de carga de página.

Abra el archivo de aplicación\_Start/BundleConfig.cs. Agregue el código siguiente al método RegisterBundles.

[!code-csharp[Main](part-6/samples/sample3.cs)]

>[!div class="step-by-step"]
[Anterior](part-5.md)
[Siguiente](part-7.md)
