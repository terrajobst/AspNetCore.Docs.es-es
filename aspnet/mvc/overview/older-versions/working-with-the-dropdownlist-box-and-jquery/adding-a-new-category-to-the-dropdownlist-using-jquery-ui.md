---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/adding-a-new-category-to-the-dropdownlist-using-jquery-ui
title: "Agregar una nueva categoría a los controles de DropDownList mediante jQuery UI | Documentos de Microsoft"
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2012
ms.topic: article
ms.assetid: 44aa1ac4-6ea2-48a2-972d-52710c48eae5
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/adding-a-new-category-to-the-dropdownlist-using-jquery-ui
msc.type: authoredcontent
ms.openlocfilehash: de661616ff3ca83052ae74d3ae6810d014aff764
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="adding-a-new-category-to-the-dropdownlist-using-jquery-ui"></a>Agregar una nueva categoría a los controles de DropDownList mediante jQuery UI
====================
Por [Rick Anderson](https://github.com/Rick-Anderson)

El código HTML `Select` etiqueta es ideal para presentar una lista de los datos de categoría fijo, pero a menudo necesita agregar una nueva categoría. ¿Supongamos que queremos a agregar el género "Opera" a las categorías en nuestra base de datos? En esta sección, usaremos jQuery UI para agregar un cuadro de diálogo, podemos utilizar para agregar una nueva categoría. La imagen siguiente muestra cómo se presentará la interfaz de usuario en el explorador.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image1.png)

Cuando un usuario selecciona el **agregar nueva género** vínculo, un cuadro de diálogo pide al usuario un nuevo nombre de género (y opcionalmente una descripción). La imagen siguiente muestra el **agregar género** cuadro de diálogo emergente.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image2.png)

Cuando se escriba un nuevo nombre de género y la **guardar** botón se inserta, ocurre lo siguiente:

1. Una llamada AJAX envía los datos para el método Create de la controladora de género, que guarda el género nueva en la base de datos y devuelve la nueva información de género (nombre de género e Id.) como JSON.
2. JavaScript agrega los nuevos datos de género a la lista de selección.
3. JavaScript convierte el género nuevo el elemento seleccionado.

 En la imagen siguiente, **Opera** se agrega a la base de datos y se selecciona en el **género** lista desplegable. 

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image3.png)

Abra la *Views\StoreManager\Create.cshtml* archivo y reemplace el marcado de género con lo siguiente en el código siguiente:

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample1.cshtml)]

El `_ChooseGenre` vista parcial contendrá toda la lógica para enlazar el JavaScript y jQuery usado para implementar la nueva característica de género agregar. Una vez que hemos completado el código resultará fácil de hacer lo mismo con el intérprete de interfaz de usuario.

En el Explorador de soluciones, haga clic con el *Views\StoreManager* carpeta y seleccione **agregar**, a continuación, **vista**. En el **nombre de la vista** de entrada, escriba `_ChooseGenre` , a continuación, seleccione **agregar**. Reemplace el marcado en el *Views\StoreManager\\_ChooseGenre.cshtml* archivo con lo siguiente:

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample2.cshtml)]

La primera línea declara que pasamos una `Album` como nuestro modelo exactamente la misma instrucción que se encuentra en la vista de creación del modelo. Las siguientes líneas son el **etiqueta** marcado de la aplicación auxiliar. La siguiente línea es la **DropDownList** auxiliar llama, exactamente igual que en la vista de creación original. La línea siguiente agrega un vínculo con el nombre `Add New Genre`, y estilos como un botón. La línea que contiene `ValidationMessageFor` se copia directamente desde la vista de creación. Las siguientes líneas:

[!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample3.html)]

crea un elemento div oculto, con el Id. de `genreDialog`. Usaremos jQuery para enlazar nuestro **agregar género** cuadro de diálogo con el Id. de `genreDialog` en este div. Las dos últimas etiquetas de script contienen vínculos a los archivos JavaScript que se usará para implementar la nueva característica de género agregar. El */Scripts/chooseGenre.js* archivo es siempre automáticamente en el proyecto, se examinará, más adelante en el tutorial.

Ejecute la aplicación y haga clic en el **agregar nueva género** botón. En el **agregar género** diálogo cuadro, escriba **Opera** en el **nombre** cuadro de entrada.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image4.png)

Haga clic en el **guardar** botón. Una llamada AJAX crea la categoría Opera y, a continuación, rellena la lista desplegable con Opera y establece Opera como el género seleccionado.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image5.png)

Escriba un intérprete, título y el precio, a continuación, seleccione la **crear** botón. Si escribe un precio menor que $8,99, el nuevo álbum aparecerá en la parte superior de la vista de índice. Comprobar que la nueva entrada de álbum se guardó en la base de datos.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image6.png)

Intente crear un nuevo género solo por una letra. El siguiente código en el *Models\Genre.cs* archivo establece la longitud mínima y máxima del nombre de género.

[!code-csharp[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample4.cs)]

Validación del lado cliente informa que debe especificar una cadena de entre 2 y 20 caracteres.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image7.png)

### <a name="examining-how-a-new-genre-is-added-to-the-database-and-the-select-list"></a>Examinar cómo un género nueva se agrega a la base de datos y la lista Select.

Abra la *Scripts\chooseGenre.js* de archivos y examine el código.

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample5.js)]

La segunda línea usa el identificador de `genreDialog` para crear un cuadro de diálogo a la etiqueta div en la *Views\StoreManager\\_ChooseGenre.cshtml* archivo. La mayoría de los parámetros con nombre es autoexplicativos. El `autoOpen` parámetro se establece en false, al seleccionar la **crear género** botón abrirá el cuadro de diálogo explícitamente (se describe esta última en). El cuadro de diálogo tiene dos botones, **guardar** y **cancelar**. El **cancelar** botón cierra el cuadro de diálogo. El código siguiente muestra el **guardar** botón función.

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample6.js)]

El `var createGenreForm` está seleccionado en el `createGenreForm` identificador. El `createGenreForm` Id. se establece en el código siguiente se encuentra en la *Views\Genre\\_CreateGenre.cshtml* archivo.

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample7.cshtml)]

El [Html.BeginForm](https://msdn.microsoft.com/library/dd492714.aspx) sobrecarga auxiliar utilizada en el *Views\Genre\\_CreateGenre.cshtml* HTML genera el archivo con un atributo de acción que contiene la dirección URL para enviar el formulario. Puede ver esto mostrando la página de álbum de crear en un explorador y seleccionar origen de mostrar en el explorador. El marcado siguiente muestra el código HTML generado que contiene la etiqueta de formulario.

[!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample8.html)]

JQuery `$.post` línea realiza una llamada de AJAX para el atributo de acción (`/StoreManager/Create`) y pasa los datos de la **crear género** cuadro de diálogo. Los datos se componen del nombre para el nuevo género y una descripción opcional. Si la llamada de AJAX es correcta, el nuevo nombre de género y el valor se agregan en el marcado de selección y el género nueva está establecido en el valor seleccionado. Como este es el marcado generado dinámicamente, no puede ver la nueva opción de seleccionar en el código fuente en el explorador. Puede ver el nuevo código HTML con las herramientas de desarrollo F12 de Internet Explorer 9. Para ver la opción de seleccionar nuevo, en Internet Explorer 9, presionar la tecla F12 para iniciar las herramientas de desarrollo F12. Vaya a la página Crear y agregar un género nueva para que el género nuevo está seleccionado en la lista de selección de género. En las herramientas de desarrollo F12:

1. Seleccione la ficha HTML.
2. Alcanzó el icono de actualización.  
    ![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image8.png)
3. En el cuadro de búsqueda, escriba GenreID.
4. Utilizando el icono siguiente,   
    ![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image9.png)  
 Desplácese hasta la siguiente etiqueta select:

    [!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample9.html)]
5. Expanda el último valor de opción.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image10.png)

El siguiente código en el *Scripts\chooseGenre.js* archivo muestra cómo el **agregar nueva género** botón obtiene conectado al evento de clic y cómo el **agregar nueva género** cuadro de diálogo creado.

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample10.js)]

La primera línea crea una función de haga clic en el **agregar nueva género** botón. El siguiente marcado de la Views\StoreManager\\_ChooseGenre.cshtml archivo muestra cómo el **agregar nueva género** botón se crea:

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample11.cshtml)]

El método load crea y abre el cuadro de diálogo Agregar género y llama jQuery `parse` método por lo que se produce la validación de cliente en los datos escritos en el cuadro de diálogo.

En esta sección ha aprendido cómo crear un cuadro de diálogo que puede usarse para agregar nuevos datos de categoría a una lista de selección. Puede seguir el mismo procedimiento para crear la interfaz de usuario para agregar a un intérprete nuevo a la lista de selección de intérprete. Este tutorial ha proporcionado información general sobre cómo trabajar con la aplicación auxiliar HTML de MVC de ASP.NET **DropDownList**. Para obtener más información sobre cómo trabajar con el **DropDownList**, vea la sección de referencias de suma más adelante. Háganoslo saber si este tutorial ha sido útil.

Rick.Anderson[at]Microsoft.com

### <a name="additional-references"></a>Referencias adicionales

- [Lista desplegable – en cascada MVC de ASP.NET muestra los Tutorial](https://weblogs.asp.net/raduenuca/archive/2011/03/06/asp-net-mvc-cascading-dropdown-lists-tutorial-part-1-defining-the-problem-and-the-context.aspx) por [Radu Enuca](https://weblogs.asp.net/raduenuca/default.aspx)
- [Elegido](http://harvesthq.github.com/chosen/) JavaScript de un complemento que admiten la selección múltiple y filtrado.

### <a name="contributors"></a>Colaboradores

- [Radu Enuca](https://weblogs.asp.net/raduenuca/default.aspx)
- Jean-Sébastien Goupil
- [Brad Wilson](http://bradwilson.typepad.com/)

### <a name="reviewers"></a>Revisores

- Jean-Sébastien Goupil
- [Brad Wilson](http://bradwilson.typepad.com/)
- Mike Pope
- Tom Dykstra

>[!div class="step-by-step"]
[Anterior](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper.md)
