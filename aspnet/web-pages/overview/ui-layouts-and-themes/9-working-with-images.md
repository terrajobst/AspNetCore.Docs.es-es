---
uid: web-pages/overview/ui-layouts-and-themes/9-working-with-images
title: Trabajar con imágenes en un sitio ASP.NET Web Pages (Razor) | Microsoft Docs
author: Rick-Anderson
description: Este capítulo muestra cómo agregar, mostrar y manipular imágenes (cambiar el tamaño, voltear y añadir marcas de agua) en su sitio Web.
ms.author: riande
ms.date: 02/20/2014
ms.assetid: 778c4e58-4372-4d25-bab9-aec4a8d8e38d
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/9-working-with-images
msc.type: authoredcontent
ms.openlocfilehash: 7536f71eb9afce9d7c8bb7e4d6326d280658c27b
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021724"
---
<a name="working-with-images-in-an-aspnet-web-pages-razor-site"></a>Trabajar con imágenes en un sitio Web de ASP.NET Pages (Razor)
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> En este artículo se muestra cómo agregar, mostrar y manipular imágenes (cambiar el tamaño, voltear y añadir marcas de agua) en un sitio Web de ASP.NET Web Pages (Razor).
> 
> Lo que aprenderá:
> 
> - Cómo agregar una imagen a una página dinámicamente.
> - Cómo permitir que los usuarios carguen una imagen.
> - Cómo cambiar el tamaño de una imagen.
> - Cómo girar o voltear una imagen.
> - Cómo agregar una marca de agua a una imagen.
> - Aprenda a utilizar una imagen como una marca de agua.
> 
> Estas son las características introducidas en el artículo de programación de ASP.NET:
> 
> - El `WebImage` auxiliar.
> - El `Path` object, que proporciona métodos que permiten manipular los nombres de archivo y ruta de acceso.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software que se usa en el tutorial
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - WebMatrix 2
>   
> 
> Este tutorial también funciona con WebMatrix 3.


<a id="Adding_an_Image"></a>
## <a name="adding-an-image-to-a-web-page-dynamically"></a>Agregar una imagen a una página Web de forma dinámica

Puede agregar imágenes a su sitio Web y a las páginas individuales al desarrollar el sitio Web. También puede permitir que los usuarios cargar imágenes, que pueden ser útiles para tareas como lo que permite agregar una foto de perfil.

Si ya está disponible una imagen en su sitio y desea mostrarlo en una página, utilice un elemento HTML `<img>` elemento similar al siguiente:

[!code-html[Main](9-working-with-images/samples/sample1.html)]

A veces, sin embargo, debe ser capaz de mostrar imágenes dinámicamente &#8212; es decir, no sabe lo que la imagen para mostrar hasta que la página se está ejecutando.

El procedimiento descrito en esta sección muestra cómo mostrar una imagen sobre la marcha donde los usuarios especificar el nombre de archivo de imagen de una lista de nombres de imagen. Seleccionan el nombre de la imagen de una lista desplegable y, al enviar la página, se muestra la imagen seleccionada.

![[image]](9-working-with-images/_static/image1.jpg "ch9images 1.jpg")

1. En WebMatrix, cree un nuevo sitio Web.
2. Agregar una nueva página denominada *DynamicImage.cshtml*.
3. En la carpeta raíz del sitio Web, agregue una nueva carpeta y asígnele el nombre *imágenes*.
4. Agregar imágenes de cuatro a la *imágenes* carpeta recién creada. (Las imágenes tenga útil hacer, pero deben caber en una página). Cambiar el nombre de las imágenes *Photo1.jpg*, *Photo2.jpg*, *Photo3.jpg*, y *Photo4.jpg*. (No usas *Photo4.jpg* en este procedimiento, pero usará más adelante en el artículo.)
5. Compruebe que las cuatro imágenes no están marcadas como de solo lectura.
6. Reemplace el contenido existente en la página con lo siguiente:

    [!code-cshtml[Main](9-working-with-images/samples/sample2.cshtml)]

    El cuerpo de la página tiene una lista desplegable (una `<select>` elemento) que se denomina `photoChoice`. La lista tiene tres opciones y el `value` atributo de cada opción de lista tiene el nombre de una de las imágenes que se coloca en el *imágenes* carpeta. En esencia, la lista permite al usuario seleccionar un nombre descriptivo como &quot;Photo 1&quot;, y, a continuación, pasa el *.jpg* nombre de archivo cuando se envía la página.

    En el código, puede obtener la selección del usuario (en otras palabras, el nombre de archivo de imagen) de la lista mediante la lectura de `Request["photoChoice"]`. Aparece en primer lugar si hay una selección. Si no existe, construir una ruta de acceso para la imagen que se compone del nombre de la carpeta para las imágenes y nombre de archivo de imagen del usuario. (Si se ha intentado crear una ruta de acceso, pero no existía nada en `Request["photoChoice"]`, obtendría un error.) Esto da como resultado una ruta de acceso relativa similar al siguiente:

    *imágenes/Photo1.jpg*

    La ruta de acceso se almacena en la variable denominada `imagePath` que necesitará más adelante en la página.

    En el cuerpo, también hay un `<img>` elemento que se usa para mostrar la imagen que se ha seleccionado el usuario. El `src` atributo no está establecido en un nombre de archivo o la dirección URL, tal como se haría para mostrar un elemento estático. En su lugar, se establece en `@imagePath`, lo que significa que obtiene su valor de la ruta de acceso que configuró en el código.

    La primera vez que se ejecuta la página, sin embargo, hay ninguna imagen para mostrar, porque el usuario no ha seleccionado nada. Esto normalmente significa que el `src` atributo estarán vacío y la imagen se debe mostrar como una roja &quot;x&quot; (o lo que representa el explorador cuando no se puede encontrar una imagen). Para evitar esto, coloca el `<img>` elemento en un `if` bloque que se comprueba para ver si el `imagePath` variable tiene nada en él. Si el usuario realiza una selección, `imagePath` contiene la ruta de acceso. Si el usuario no ha seleccionado una imagen o si se trata de la primera vez que se muestra la página, el `<img>` aún no se procesa el elemento.
7. Guarde el archivo y ejecute la página en un explorador. (Asegúrese de que la página está seleccionada en el **archivos** área de trabajo antes de ejecutarlo.)
8. Seleccione una imagen de la lista desplegable y, a continuación, haga clic en **imagen de ejemplo**. Asegúrese de que ve diferentes imágenes para las diferentes opciones.

<a id="Uploading_an_Image"></a>
## <a name="uploading-an-image"></a>Cargar una imagen

El ejemplo anterior mostró cómo mostrar una imagen de forma dinámica, pero ha funcionado sólo con imágenes que ya estaban en su sitio Web. Este procedimiento muestra cómo permitir que los usuarios carguen una imagen, que, a continuación, se muestra en la página. En ASP.NET, puede manipular imágenes sobre la marcha mediante la `WebImage` aplicación auxiliar, que tiene métodos que permiten crear, manipular y guardar las imágenes. El `WebImage` auxiliar es compatible con todas los web imagen tipos de archivo comunes, incluidos *.jpg*, *.png*, y *.bmp*. En este artículo, usará *.jpg* imágenes, pero puede usar cualquiera de los tipos de imagen.

![[image]](9-working-with-images/_static/image2.jpg "ch9images 2.jpg")

1. Agregue una nueva página y asígnele el nombre *UploadImage.cshtml*.
2. Reemplace el contenido existente en la página con lo siguiente: 

    [!code-cshtml[Main](9-working-with-images/samples/sample3.cshtml)]

    El cuerpo del texto tiene un `<input type="file">` elemento, que permite a los usuarios seleccionar un archivo para cargarlo. Al hacer clic **enviar**, el archivo seleccionados por el que se envía junto con el formulario.

    Para obtener la imagen cargada, se utiliza el `WebImage` auxiliar, que tiene todo tipo de métodos útiles para trabajar con imágenes. En concreto, usa `WebImage.GetImageFromRequest` para obtener la imagen cargada (si existe) y almacenarlo en una variable denominada `photo`.

    Una gran parte del trabajo en este ejemplo implica obtener y establecer los nombres de archivo y ruta de acceso. El problema es que desea obtener el nombre (y solo el nombre) de la imagen que el usuario ha cargado y, a continuación, cree una nueva ruta de acceso donde va a almacenar la imagen. Dado que los usuarios potencialmente podrían cargar varias imágenes que tienen el mismo nombre, utilice un poco de código adicional para crear nombres únicos y asegúrese de que los usuarios no sobrescriban las imágenes existentes.

    Si una imagen se ha cargado realmente (la prueba `if (photo != null)`), obtendrá el nombre de la imagen de la imagen `FileName` propiedad. Cuando el usuario carga la imagen, `FileName` contiene el nombre del usuario original, que incluye la ruta de acceso desde el equipo del usuario. Podría parecerse a esto:

    *C:\Users\Joe\Pictures\SamplePhoto1.jpg*

    No desea que esa información de ruta de acceso, aunque &#8212; tan solo quiere el nombre de archivo real (*SamplePhoto1.jpg*). Puede eliminar solo el archivo desde una ruta de acceso mediante el uso de la `Path.GetFileName` método, similar al siguiente:

    [!code-csharp[Main](9-working-with-images/samples/sample4.cs)]

    A continuación, creará un nuevo nombre de archivo único mediante la adición de un GUID al nombre original. (Para obtener más información acerca de los GUID, consulte [sobre GUID](#SB_AboutGUIDs) más adelante en este artículo.) A continuación, construir una ruta de acceso completa que puede usar para guardar la imagen. Guardar ruta de acceso está formado por el nuevo nombre de archivo, la carpeta (imágenes) y la ubicación del sitio Web actual.

    > [!NOTE]
    > Para el código guardar los archivos en el *imágenes* carpeta, la aplicación necesita permisos de lectura-escritura para esa carpeta. En el equipo de desarrollo esto no es normalmente un problema. Sin embargo, cuando se publica un sitio al servidor de un proveedor de hospedaje web, debe establecer explícitamente esos permisos. Si ejecuta este código en el servidor de un proveedor de hospedaje y obtiene errores, póngase en contacto con el proveedor de hospedaje para obtener información sobre cómo establecer estos permisos.

    Por último, se pasa al guardar ruta de acceso a la `Save` método de la `WebImage` auxiliar. Almacena la imagen cargada en su nuevo nombre. La operación de guardar método tiene este aspecto: `photo.Save(@"~\" + imagePath)`. La ruta de acceso completa se anexa a `@"~\"`, que es la ubicación del sitio Web actual. (Para obtener información sobre la `~` operador, consulte [Introducción a ASP.NET Web programación mediante la sintaxis Razor](https://go.microsoft.com/fwlink/?LinkId=202890#ID_WorkingWithFileAndFolderPaths).)

    Como se muestra en el ejemplo anterior, el cuerpo de la página contiene un `<img>` elemento para mostrar la imagen. Si `imagePath` se ha establecido, el `<img>` se representa el elemento y su `src` atributo está establecido en el `imagePath` valor.
3. Ejecute la página en un explorador.
4. Cargar una imagen y asegúrese de que se muestra en la página.
5. En su sitio, abra el *imágenes* carpeta. Verá que se ha agregado un nuevo archivo cuyo nombre de archivo es algo parecido a esto: 

    *45ea4527-7ddd-4965-b9ca-c6444982b342\_MyPhoto.png*

    Esta es la imagen que ha cargado con un GUID como precedido al nombre. (Su propio archivo tendrá un GUID diferente y probablemente se denomina algo distinto a *MyPhoto.png*.)

> [!TIP] 
> 
> <a id="SB_AboutGUIDs"></a>
> ### <a name="about-guids"></a>Acerca de los GUID
> 
> Un GUID (identificador único global) es un identificador que normalmente se representa en un formato similar al siguiente: `936DA01F-9ABD-4d9d-80C7-02AF85C822A8`. Los números y letras (de A-F) distinto para cada GUID, pero siguen el patrón de uso de grupos de caracteres de 8-4-4-4-12. (Técnicamente, un GUID es un número de 16 bytes o 128 bits). Cuando se necesita un GUID, puede llamar a código especializado que genera un GUID para usted. La idea detrás de los GUID es que entre el tamaño de la cantidad enorme (3,4 x 10<sup>38</sup>) y el algoritmo para que lo genera, el número resultante es prácticamente garantiza que uno de cada tipo. Por lo tanto, los GUID son una buena forma de generar los nombres de las cosas cuando debe garantizar que no use el mismo nombre dos veces. La desventaja, por supuesto, es que los GUID no son especialmente descriptivos, por lo que tienden a utilizarse cuando se utiliza el nombre en el código.


<a id="Resizing_an_Image"></a>
## <a name="resizing-an-image"></a>Cambiar el tamaño de una imagen

Si su sitio Web acepta las imágenes de un usuario, es posible que desee cambiar el tamaño de las imágenes antes de mostrar o guardarlos. Puede usar nuevamente el `WebImage` auxiliar para esto.

Este procedimiento muestra cómo cambiar el tamaño de una imagen cargada para crear una miniatura y, a continuación, guarde la imagen original y la miniatura en el sitio Web. Mostrar la miniatura en la página y utilizar un hipervínculo para redirigir a los usuarios a la imagen en tamaño completo.

![[image]](9-working-with-images/_static/image3.jpg "ch9images 3.jpg")

1. Agregar una nueva página denominada *Thumbnail.cshtml*.
2. En el *imágenes* carpeta, cree una subcarpeta denominada *thumbs*.
3. Reemplace el contenido existente en la página con lo siguiente: 

    [!code-cshtml[Main](9-working-with-images/samples/sample5.cshtml)]

    Este código es similar al código del ejemplo anterior. La diferencia es que este código guarda la imagen dos veces, una vez normalmente y una vez después de crear una copia de la imagen en miniatura. En primer lugar Obtenga la imagen cargada y guárdelo en el *imágenes* carpeta. A continuación, crear una nueva ruta de acceso para la imagen en miniatura. Para crear realmente la miniatura, llame a la `WebImage` la aplicación auxiliar `Resize` método para crear una imagen de píxel de 60 x 60 píxeles. El ejemplo muestra cómo se conserva la relación de aspecto y cómo puede impedir que la imagen que se amplía (en caso de que el nuevo tamaño realmente haría que la imagen más grande). A continuación, se guarda la imagen cuyo tamaño ha cambiado en el *thumbs* subcarpeta.

    Al final del marcado, use el mismo `<img>` elemento con el dinámico `src` atributo que ha visto en los ejemplos anteriores para mostrar condicionalmente la imagen. En este caso, mostrar la miniatura. También usa un `<a>` elemento para crear un hipervínculo a la versión de la imagen grande. Igual que con el `src` atributo de la `<img>` elemento, Establece el `href` atributo de la `<a>` elemento dinámicamente a cualquier cosa que esté en `imagePath`. Para asegurarse de que la ruta de acceso puede funcionar como una dirección URL, pasa `imagePath` a la `Html.AttributeEncode` método, que convierte los caracteres reservados en la ruta de acceso a los caracteres que no importa en una dirección URL.
4. Ejecute la página en un explorador.
5. Cargar una foto y compruebe que aparece la miniatura.
6. Haga clic en la miniatura para ver la imagen en tamaño completo.
7. En el *imágenes* y *imágenes/thumbs*, tenga en cuenta que se han agregado nuevos archivos.

<a id="Rotating_and_Flipping"></a>
## <a name="rotating-and-flipping-an-image"></a>Girar y voltear una imagen

El `WebImage` auxiliar también permite voltear y rotar imágenes. Este procedimiento muestra cómo obtener una imagen desde el servidor, la imagen se voltea boca abajo (vertical), guárdelo y, a continuación, se mostrará la imagen volteada en la página. En este ejemplo, solo usa un archivo que ya tiene en el servidor (*Photo2.jpg*). En una aplicación real, probablemente sería voltear una imagen cuyo nombre se obtiene de forma dinámica, como hizo en los ejemplos anteriores.

![[image]](9-working-with-images/_static/image4.jpg "ch9images 4.jpg")

1. Agregar una nueva página denominada *FlipImage.cshtml*.
2. Reemplace el contenido existente en la página con lo siguiente: 

    [!code-cshtml[Main](9-working-with-images/samples/sample6.cshtml)]

    El código usa el `WebImage` auxiliar para obtener una imagen desde el servidor. Crear la ruta de acceso a la imagen con la misma técnica usada en ejemplos anteriores para guardar las imágenes y pasar esa ruta de acceso al crear una imagen mediante `WebImage`:

    [!code-javascript[Main](9-working-with-images/samples/sample7.js)]

    Si se encuentra una imagen, crear una nueva ruta de acceso y nombre, como hizo en los ejemplos anteriores. Para voltear la imagen, se llama a la `FlipVertical` método y, a continuación, vuelva a guarda la imagen.

    La imagen aparece de nuevo en la página mediante el uso de la `<img>` elemento con el `src` atributo establecido en `imagePath`.
3. Ejecute la página en un explorador. La imagen para *Photo2.jpg* se muestra boca abajo.
4. Actualice la página o solicite la página nuevo para ver que la imagen es volteado lado derecho de nuevo.

Para girar una imagen, usa el mismo código, salvo que en lugar de llamar el `FlipVertical` o `FlipHorizontal`, se llama a `RotateLeft` o `RotateRight`.

<a id="Adding_a_Watermark"></a>
## <a name="adding-a-watermark-to-an-image"></a>Agregar una marca de agua a una imagen

Al agregar imágenes a su sitio Web, es posible que desee agregar una marca de agua a la imagen antes de guardarla o mostrarla en una página. Las personas a menudo usan marcas de agua para agregar información de copyright a una imagen o para anunciar su nombre de la empresa.

![[image]](9-working-with-images/_static/image5.jpg "ch9images 5.jpg")

1. Agregar una nueva página denominada *Watermark.cshtml*.
2. Reemplace el contenido existente en la página con lo siguiente: 

    [!code-cshtml[Main](9-working-with-images/samples/sample8.cshtml)]

    Este código es similar al código de la *FlipImage.cshtml* página anterior (aunque esta ocasión, utilizará el *Photo3.jpg* archivo). Para agregar la marca de agua, llame a la `WebImage` la aplicación auxiliar `AddTextWatermark` método antes de guardar la imagen. En la llamada a `AddTextWatermark`, pasar el texto &quot;mi marca de agua&quot;, establezca el color de fuente en amarillo y establecer la familia de fuentes Arial. (Aunque no se muestra en este caso, el `WebImage` auxiliar también le permite especificar la opacidad, familia de fuentes y tamaño de fuente y la posición del texto de marca de agua.) Cuando se guarda la imagen no debe ser de solo lectura.

    Como ha visto antes, la imagen se muestra en la página mediante el uso de la `<img>` elemento con el atributo src establecido en `@imagePath`.
3. Ejecute la página en un explorador. Observe el texto "Mi marca de agua" en la esquina inferior derecha de la imagen.

<a id="Using_an_Image_as_a_Watermark"></a>
## <a name="using-an-image-as-a-watermark"></a>Uso de una imagen como una marca de agua

En lugar de texto para una marca de agua, puede usar otra imagen. Personas a veces usan imágenes como un logotipo de empresa como una marca de agua o utilizar una imagen de marca de agua en lugar de texto de información de copyright.

![[image]](9-working-with-images/_static/image6.jpg "ch9images 6.jpg")

1. Agregar una nueva página denominada *ImageWatermark.cshtml*.
2. Agregar una imagen a la *imágenes* carpeta que puede usar como un logotipo y cambiar el nombre de la imagen *MyCompanyLogo.jpg*. Esta imagen debe ser una imagen que se puede ver con claridad cuando se establece en 80 píxeles de ancho por 20 píxeles de alto.
3. Reemplace el contenido existente en la página con lo siguiente: 

    [!code-cshtml[Main](9-working-with-images/samples/sample9.cshtml)]

    Esta es otra variación en el código de los ejemplos anteriores. En este caso, se llama a `AddImageWatermark` para agregar la imagen de marca de agua en la imagen de destino (*Photo3.jpg*) antes de guardar la imagen. Cuando se llama a `AddImageWatermark`, establezca el ancho en 80 píxeles y el alto a 20 píxeles. El *MyCompanyLogo.jpg* imagen se alinea horizontalmente en el centro y se alinea verticalmente en la parte inferior de la imagen de destino. La opacidad se establece en 100% y el relleno se establece en 10 píxeles. Si la imagen de marca de agua es mayor que la imagen de destino, no sucederá nada. Si la imagen de marca de agua es mayor que la imagen de destino y establecer el relleno de la marca de agua de imagen en cero, se omitirá la marca de agua.

    Como antes, mostrar la imagen mediante el `<img>` elemento y una dinámica `src` atributo.
4. Ejecute la página en un explorador. Observe que aparece la imagen de marca de agua en la parte inferior de la imagen principal.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Recursos adicionales


[Trabajar con archivos en un sitio de ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202896)

[Introducción a ASP.NET Web Pages usando la sintaxis Razor de programación](https://go.microsoft.com/fwlink/?LinkID=251587)
