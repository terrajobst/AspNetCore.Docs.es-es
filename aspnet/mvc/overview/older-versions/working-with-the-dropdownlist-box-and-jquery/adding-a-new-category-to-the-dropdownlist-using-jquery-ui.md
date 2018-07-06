---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/adding-a-new-category-to-the-dropdownlist-using-jquery-ui
title: Agregar una nueva categoría al control DropDownList mediante jQuery UI | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
ms.date: 01/12/2012
ms.assetid: 44aa1ac4-6ea2-48a2-972d-52710c48eae5
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/adding-a-new-category-to-the-dropdownlist-using-jquery-ui
msc.type: authoredcontent
ms.openlocfilehash: e2528b74b714a3f691b07ed2429b3fe9eb3c2074
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37802531"
---
<a name="adding-a-new-category-to-the-dropdownlist-using-jquery-ui"></a>Agregar una nueva categoría al control DropDownList mediante jQuery UI
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

El código HTML `Select` etiqueta es ideal para presentar una lista de los datos de categoría fijo, pero a veces tiene que agregar una nueva categoría. ¿Supongamos que queremos agregar el género "Opera" a las categorías en nuestra base de datos? En esta sección, usaremos jQuery UI para agregar un cuadro de diálogo, que podemos usar para agregar una nueva categoría. La imagen siguiente muestra cómo se presentará la interfaz de usuario en el explorador.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image1.png)

Cuando un usuario selecciona el **Agregar nuevo género** vínculo, un cuadro de diálogo pide al usuario para un nuevo nombre de género (y opcionalmente una descripción). La imagen siguiente muestra el **agregar género** cuadro de diálogo emergente.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image2.png)

Cuando se introduce un nuevo nombre de género y el **guardar** botón está presionado, ocurre lo siguiente:

1. Una llamada AJAX envía los datos para el método Create del controlador de género, que guarda el género nuevo en la base de datos y devuelve la nueva información de género (nombre de género e Id.) como JSON.
2. JavaScript agrega los nuevos datos de género a la lista de selección.
3. JavaScript convierte el género nuevo el elemento seleccionado.

   En la imagen siguiente, **Opera** se agrega a la base de datos y se selecciona en el **género** lista desplegable. 

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image3.png)

Abrir el *Views\StoreManager\Create.cshtml* de archivo y reemplace el marcado de género con lo siguiente en el código siguiente:

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample1.cshtml)]

El `_ChooseGenre` vista parcial contendrá toda la lógica para enlazar el JavaScript y jQuery usado para implementar la nueva característica de género agregar. Una vez que hemos completado el código será fácil de hacer lo mismo con el intérprete de la interfaz de usuario.

En el Explorador de soluciones, haga clic la *Views\StoreManager* carpeta y seleccione **agregar**, a continuación, **vista**. En el **nombre de la vista** de entrada, escriba `_ChooseGenre` , a continuación, seleccione **agregar**. Reemplace el marcado en el *Views\StoreManager\\_ChooseGenre.cshtml* archivo por lo siguiente:

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample2.cshtml)]

La primera línea declara que pasamos un `Album` como nuestro modelo, exactamente la misma instrucción se encuentra en la vista de creación del modelo. Las siguientes líneas son la **etiqueta** marcado de aplicación auxiliar. La siguiente línea es la **DropDownList** auxiliar llama, exactamente igual que en la vista de creación original. La siguiente línea agrega un vínculo con el nombre `Add New Genre`, y determina el estilo como un botón. La línea que contiene `ValidationMessageFor` se copian directamente desde la vista de creación. Las siguientes líneas:

[!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample3.html)]

crea un elemento div oculto, con el Id. de `genreDialog`. Usaremos jQuery para enlazar nuestras **agregar género** cuadro de diálogo con el Id. de `genreDialog` en este div. Las dos últimas etiquetas de script contienen vínculos a los archivos de JavaScript que se usará para implementar la nueva característica de género agregar. El */Scripts/chooseGenre.js* archivo es proporcionada por usted en el proyecto, examinaremos, más adelante en el tutorial.

Ejecute la aplicación y haga clic en el **Agregar nuevo género** botón. En el **agregar género** diálogo cuadro, escriba **Opera** en el **nombre** cuadro de entrada.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image4.png)

Haga clic en el botón **Guardar**. Una llamada AJAX crea la categoría Opera y, a continuación, rellena la lista desplegable con Opera y también establece Opera como el género seleccionado.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image5.png)

Escriba un artista, título y el precio, y después seleccione el **crear** botón. Si escribe un precio inferior a $8,99, el nuevo álbum aparecerá en la parte superior de la vista de índice. Compruebe que la nueva entrada de álbum se guardó en la base de datos.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image6.png)

Intente crear un nuevo género con solo una letra. El siguiente código en el *Models\Genre.cs* archivo establece la longitud mínima y máxima del nombre del género.

[!code-csharp[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample4.cs)]

Validación del lado cliente informa de que debe escribir una cadena de entre 2 y 20 caracteres.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image7.png)

### <a name="examining-how-a-new-genre-is-added-to-the-database-and-the-select-list"></a>Examinar un nuevo género cómo se agrega a la base de datos y la lista Select.

Abra el *Scripts\chooseGenre.js* de archivo y examine el código.

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample5.js)]

La segunda línea usa el identificador de `genreDialog` para crear un cuadro de diálogo en la etiqueta div en la *Views\StoreManager\\_ChooseGenre.cshtml* archivo. La mayoría de los parámetros con nombre son autoexplicativos. El `autoOpen` parámetro se establece en false, seleccionando la **crear género** botón, abre el cuadro de diálogo explícitamente (se describe este último en). El cuadro de diálogo tiene dos botones, **guardar** y **cancelar**. El **cancelar** botón cierra el cuadro de diálogo. El siguiente código muestra la **guardar** botón de función.

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample6.js)]

El `var createGenreForm` está seleccionado en el `createGenreForm` identificador. El `createGenreForm` ID se estableció en el siguiente código se encuentra en la *Views\Genre\\_CreateGenre.cshtml* archivo.

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample7.cshtml)]

El [Html.BeginForm](https://msdn.microsoft.com/library/dd492714.aspx) sobrecarga auxiliar utilizada en el *Views\Genre\\_CreateGenre.cshtml* genera el archivo HTML con un atributo de acción que contiene la dirección URL para enviar el formulario. Puede ver esto mostrando la página de álbum de crear en un explorador y seleccionar origen de presentación en el explorador. El marcado siguiente muestra el código HTML generado que contiene la etiqueta de formulario.

[!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample8.html)]

JQuery `$.post` línea realiza una llamada de AJAX para el atributo de acción (`/StoreManager/Create`) y pasa los datos de la **crear género** cuadro de diálogo. Los datos se componen del nombre para el nuevo género y una descripción opcional. Si la llamada de AJAX es correcta, el nuevo nombre de género y el valor se agregan al marcado de la selección y el género nueva está establecido en el valor seleccionado. Dado que esto es marcado generado de forma dinámica, no puede ver la nueva opción de seleccionar en el código fuente en el explorador. Puede ver el código HTML nuevo con las herramientas de desarrollo F12 de Internet Explorer 9. Para ver la nueva opción de seleccionar, en Internet Explorer 9, presionar la tecla F12 para iniciar las herramientas de desarrollo F12. Vaya a la página Crear y agregar un nuevo género, por lo que está seleccionado el género nuevo en la lista de selección de género. En las herramientas de desarrollo F12:

1. Seleccione la ficha HTML.
2. Presione el icono de actualización.  
    ![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image8.png)
3. En el cuadro de búsqueda, escriba GenreID.
4. Mediante el icono siguiente,   
    ![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image9.png)  
   Vaya a la etiqueta select siguiente:

    [!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample9.html)]
5. Expanda el último valor de opción.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image10.png)

El siguiente código en el *Scripts\chooseGenre.js* archivo muestra cómo el **Agregar nuevo género** botón obtiene conectado al evento de clic y cómo el **Agregar nuevo género** cuadro de diálogo creado.

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample10.js)]

La primera línea crea una función de clic conectada a la **Agregar nuevo género** botón. El siguiente marcado de la Views\StoreManager\\_ChooseGenre.cshtml archivo muestra cómo el **Agregar nuevo género** crea botón:

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample11.cshtml)]

El método de carga crea y abre el cuadro de diálogo Agregar género y llama a jQuery `parse` método por lo que se produce la validación de cliente de los datos introducidos en el cuadro de diálogo.

En esta sección ha aprendido a crear un cuadro de diálogo que se puede utilizar para agregar nuevos datos de categoría a una lista de selección. Puede seguir el mismo procedimiento para crear la interfaz de usuario para agregar a un nuevo intérprete a la lista de selección del artista. Este tutorial le haya proporcionado una visión general de cómo trabajar con la aplicación auxiliar HTML de ASP.NET MVC **DropDownList**. Para obtener más información sobre cómo trabajar con el **DropDownList**, vea la siguiente sección de referencias de adición. Háganoslo saber si este tutorial ha sido útil.

Rick.Anderson[at]Microsoft.com

### <a name="additional-references"></a>Referencias adicionales

- [ASP.NET MVC: Cascading desplegable enumera Tutorial](https://weblogs.asp.net/raduenuca/archive/2011/03/06/asp-net-mvc-cascading-dropdown-lists-tutorial-part-1-defining-the-problem-and-the-context.aspx) por [Radu Enuca](https://weblogs.asp.net/raduenuca/default.aspx)
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

> [!div class="step-by-step"]
> [Anterior](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper.md)
