---
uid: web-pages/overview/ui-layouts-and-themes/3-creating-a-consistent-look
title: Crear un diseño homogéneo en ASP.NET Web Pages (Razor) sitios | Documentos de Microsoft
author: tfitzmac
description: Para que resulte más eficaz para crear páginas web para su sitio, puede crear bloques reutilizables de contenido (por ejemplo, los encabezados y pies de página) para su sitio Web y c...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/10/2014
ms.topic: article
ms.assetid: d7bd001b-6db2-4422-9b78-f3d08b743b00
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/3-creating-a-consistent-look
msc.type: authoredcontent
ms.openlocfilehash: 2c7631017f7c0fb31f43320c2ab78baddd87b516
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
ms.locfileid: "26530244"
---
<a name="creating-a-consistent-layout-in-aspnet-web-pages-razor-sites"></a>Crear un diseño homogéneo en sitios de ASP.NET Web Pages (Razor)
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Este artículo explica cómo puede usar páginas de diseño en un sitio Web de ASP.NET Web Pages (Razor) para crear bloques reutilizables de contenido (por ejemplo, los encabezados y pies de página) y para crear una apariencia coherente para todas las páginas en el sitio.
> 
> **Lo que aprenderá:** 
> 
> - Cómo crear bloques reutilizables de contenido, como encabezados y pies de página.
> - Cómo crear una apariencia coherente para todas las páginas de su sitio mediante un diseño.
> - Cómo pasar datos en tiempo de ejecución a una página de diseño.
> 
> Estas son las características ASP.NET presentadas en el artículo:
> 
> - Bloques de contenido, que son archivos que contienen el contenido con formato HTML que deben insertarse en varias páginas.
> - Páginas de diseño, que son páginas que contienen el contenido con formato HTML que se pueden compartir por las páginas en el sitio Web.
> - El `RenderPage`, `RenderBody`, y `RenderSection` métodos, que indican dónde debe insertar los elementos de la página ASP.NET.
> - El `PageData` diccionario que le permite compartir datos entre bloques de contenido y páginas de diseño.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software que se usa en el tutorial
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> Este tutorial también funciona con ASP.NET Web Pages 2.


## <a name="about-layout-pages"></a>Acerca de las páginas de diseño

Muchos sitios Web tiene contenido que se muestra en todas las páginas, como un encabezado y pie de página o un cuadro que indica a los usuarios que han iniciado sesión. ASP.NET le permite crear un archivo independiente con un bloque de contenido que puede contener texto, marcado y código, al igual que una página web normal. A continuación, puede insertar el bloque de contenido en otras páginas en el sitio donde desea que aparezca la información. De este modo que no tiene que copiar y pegar el mismo contenido en cada página. Crear contenido común parecido a esto también resulta más fácil actualizar el sitio. Si necesita cambiar el contenido, solo puede actualizar un único archivo y, a continuación, se reflejan los cambios en cualquier lugar se ha insertado el contenido.

El siguiente diagrama muestra cómo contenido bloquea el trabajo. Cuando un explorador solicita una página del servidor web, ASP.NET inserta los bloques de contenido en el punto donde la `RenderPage` método se llama en la página principal. La página final (combinada), a continuación, se envía al explorador.

![Diagrama conceptual que muestra cómo el método RenderPage inserta una página que se hace referencia en la página actual.](3-creating-a-consistent-look/_static/image1.jpg)

En este procedimiento, creará una página que hace referencia a dos bloques de contenido (un encabezado y un pie de página) que se encuentran en archivos independientes. Puede utilizar estos mismos bloques de contenido en cualquier página de su sitio. Cuando haya terminado, le mostrará una página similar al siguiente:

![Captura de pantalla que muestra una página en el explorador que es el resultado de la ejecución de una página que incluye las llamadas al método RenderPage.](3-creating-a-consistent-look/_static/image2.jpg)

1. En la carpeta raíz de su sitio Web, cree un archivo denominado *Index.cshtml*.
2. Reemplace el marcado existente con lo siguiente:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample1.html)]
3. En la carpeta raíz, cree una carpeta denominada *Shared*.

    > [!NOTE]
    > Es una práctica común para almacenar los archivos que se comparten entre páginas web en una carpeta denominada *Shared*.
4. En el *Shared* carpeta, cree un archivo denominado  *\_Header.cshtml*.
5. Reemplace el contenido existente con lo siguiente:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample2.html)]

    Observe que el nombre de archivo es  *\_Header.cshtml*, con un carácter de subrayado (\_) como prefijo. ASP.NET no enviar una página al explorador si su nombre empieza por un carácter de subrayado. Esto impide que personas solicitando (sin darse cuenta o de otro modo) estas páginas directamente. Es una buena idea usar un carácter de subrayado en las páginas de nombre que tienen bloques de contenido en ellos, ya que realmente no desea que los usuarios puedan solicitar estas páginas &#8212; Existen estrictamente se inserten en otras páginas.
6. En el *Shared* carpeta, cree un archivo denominado  *\_Footer.cshtml* y reemplace el contenido por lo siguiente:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample3.html)]
7. En el *Index.cshtml* página, agregue dos llamadas a la `RenderPage` método, como se muestra aquí:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample4.html)]

    Esto muestra cómo insertar un bloque de contenido en una página web. Se llama a la `RenderPage` método y pásele el nombre del archivo cuyo contenido desea insertar en ese momento. En este caso, se inserta el contenido de la  *\_Header.cshtml* y  *\_Footer.cshtml* de archivos en el *Index.cshtml* archivo.
8. Ejecute el *Index.cshtml* página en un explorador. (En WebMatrix, en la **archivos** área de trabajo, haga clic en el archivo y, a continuación, seleccione **iniciar en un explorador**.)
9. En el explorador, vea el origen de la página. (Por ejemplo, en Internet Explorer, haga clic en la página y, a continuación, haga clic en **ver código fuente**.)

    Esto le permite ver el marcado de la página web que se envía al explorador, que combina el marcado de la página de índice con los bloques de contenido. En el ejemplo siguiente se muestra el origen de la página que se presenta para *Index.cshtml*. Las llamadas a `RenderPage` que insertó en *Index.cshtml* han sido reemplazados por el contenido real de los archivos de encabezado y pie de página.

    [!code-html[Main](3-creating-a-consistent-look/samples/sample5.html)]

## <a name="creating-a-consistent-look-using-layout-pages"></a>Crear una apariencia coherente mediante páginas de diseño

Hasta ahora ha visto que resulta fácil de incluir el mismo contenido en varias páginas. Un enfoque más estructurado para crear una apariencia coherente para un sitio es usar páginas de diseño. Una página de diseño define la estructura de una página web, pero no contiene ningún contenido real. Después de crear una página de diseño, puede crear páginas web que contengan el contenido y, a continuación, vincúlelos a la página de diseño. Cuando se muestran estas páginas, se le aplica formato según la página de diseño. (En este sentido, una página de diseño actúa como un tipo de plantilla para el contenido que se define en otras páginas).

La página de diseño es igual que cualquier página HTML, salvo que contiene una llamada a la `RenderBody` método. La posición de la `RenderBody` método en la página de diseño determina dónde se incluirá la información de la página de contenido.

El siguiente diagrama muestra las páginas de administración de contenido y páginas de diseño se combinan en tiempo de ejecución para generar la página web finalizada. El explorador solicita una página de contenido. La página de contenido contiene código que especifica la página de diseño que se utilizará para la estructura de la página. En la página de diseño, el contenido se insertará en el punto donde la `RenderBody` se llama al método. Contenido de bloques también se pueden insertar en la página de diseño mediante una llamada a la `RenderPage` método, la manera que en la sección anterior. Una vez completada la página web, se envía al explorador.

![Captura de pantalla que muestra una página en el explorador que es el resultado de la ejecución de una página que incluye las llamadas al método RenderBody.](3-creating-a-consistent-look/_static/image3.jpg)

El siguiente procedimiento muestra cómo crear un diseño de páginas de contenido de página y un vínculo a él.

1. En el *Shared* carpeta del sitio Web, cree un archivo denominado  *\_Layout1.cshtml*.
2. Reemplace el contenido existente con lo siguiente:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample6.html)]

    Usa el `RenderPage` método en una página de diseño para insertar bloques de contenido. Una página de diseño puede contener sólo una llamada a la `RenderBody` método.
3. En el *Shared* carpeta, cree un archivo denominado  *\_Header2.cshtml* y reemplace el contenido existente con lo siguiente:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample7.html)]
4. En la carpeta raíz, cree una nueva carpeta y asígnele el nombre *estilos*.
5. En el *estilos* carpeta, cree un archivo denominado *Site.css* y agregue las siguientes definiciones de estilo:

    [!code-css[Main](3-creating-a-consistent-look/samples/sample8.css)]

    Estas definiciones de estilo aquí son solo mostrar cómo pueden usarse las hojas de estilos con páginas de diseño. Si lo desea, puede definir sus propios estilos para estos elementos.
6. En la carpeta raíz, cree un archivo denominado *Content1.cshtml* y reemplace el contenido existente con lo siguiente:

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample9.cshtml)]

    Se trata de una página que va a usar una página de diseño. El bloque de código en la parte superior de la página indica qué página de diseño se utiliza para dar formato a este contenido.
7. Ejecutar *Content1.cshtml* en un explorador. La página representada utiliza el formato y la hoja de estilos definidos en  *\_Layout1.cshtml* y el texto (contenido) definido en *Content1.cshtml*.

    ![[image]](3-creating-a-consistent-look/_static/image4.jpg)

    Puede repetir el paso 6 para crear páginas de contenido adicionales que, a continuación, se pueden compartir la misma página de diseño.

    > [!NOTE]
    > Puede configurar su sitio para que pueden usar automáticamente la misma página de diseño para todas las páginas de contenido en una carpeta. Para obtener más información, consulte [personalizar el comportamiento de todo el sitio para ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202906).

## <a name="designing-layout-pages-that-have-multiple-content-sections"></a>Diseñar páginas de diseño que tienen varias secciones de contenido

Una página de contenido puede tener varias secciones, lo que resulta útil si desea usar diseños que tienen varias áreas con contenido reemplazable. En la página de contenido, asigne cada sección de un nombre único. (Se deja la sección predeterminada sin nombre.) En la página de diseño, se agregará un `RenderBody` método para especificar dónde debe aparecer la sección sin nombre (valor predeterminado). A continuación, agregar independiente `RenderSection` métodos para representar secciones con nombre individualmente.

El siguiente diagrama muestra cómo ASP.NET controla el contenido que se divide en varias secciones. Cada sección con nombre se encuentra en un bloque de sección en la página de contenido. (Le denomina `Header` y `List` en el ejemplo.) El marco de trabajo inserta la sección de contenido en la página de diseño en el punto donde la `RenderSection` se llama al método. La sección sin nombre (valor predeterminado) se inserta en el punto donde el `RenderBody` se denomina método, como vio anteriormente.

![Diagrama conceptual que muestra cómo el método RenderSection inserta secciones de referencias en la página actual.](3-creating-a-consistent-look/_static/image5.jpg)

Este procedimiento muestra cómo crear una página de contenido que tiene varias secciones de contenido y cómo representar mediante una página de diseño que admite varias secciones de contenido.

1. En el *Shared* carpeta, cree un archivo denominado  *\_Layout2.cshtml*.
2. Reemplace el contenido existente con lo siguiente:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample10.html)]

    Usa el `RenderSection` método para representar el encabezado y la lista de secciones.
3. En la carpeta raíz, cree un archivo denominado *Content2.cshtml* y reemplace el contenido existente con lo siguiente:

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample11.cshtml)]

    Esta página de contenido contiene un bloque de código en la parte superior de la página. Cada sección con nombre se encuentra en un bloque de sección. El resto de la página contiene la sección de contenido predeterminado (sin nombre).
4. Ejecutar *Content2.cshtml* en un explorador.

    ![Captura de pantalla que muestra una página en el explorador que es el resultado de la ejecución de una página que incluye las llamadas al método RenderSection.](3-creating-a-consistent-look/_static/image6.jpg)

## <a name="making-content-sections-optional"></a>Hacer que las secciones de contenido opcionales

Normalmente, las secciones que se crean en una página de contenido tienen que coincidir con las secciones que se definen en la página de diseño. Puede obtener errores si se produce alguna de las siguientes acciones:

- La página de contenido contiene una sección con ninguna sección correspondiente en la página de diseño.
- La página de diseño contiene una sección para la que no hay ningún contenido.
- La página de diseño incluye llamadas al método que intenta representar varias veces la misma sección.

Sin embargo, puede invalidar este comportamiento para una sección con nombre mediante la declaración de la sección sea opcional en la página de diseño. Esto le permite definir varias páginas de contenido que pueden compartir una página de diseño pero que puede o no tener el contenido de una sección concreta.

1. Abra *Content2.cshtml* y quite la siguiente sección:

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample12.cshtml)]
2. Guarde la página y, a continuación, ejecútelo en un explorador. Se muestra un mensaje de error, porque la página de contenido no proporciona contenido para una sección definida en la página de diseño, es decir, la sección de encabezado.

    ![Captura de pantalla que muestra el error que se produce si se ejecuta una página que llama el método RenderSection pero no se proporciona la sección correspondiente.](3-creating-a-consistent-look/_static/image7.jpg)
3. En el *Shared* carpeta, abra el  *\_Layout2.cshtml* página y reemplace esta línea:

    [!code-javascript[Main](3-creating-a-consistent-look/samples/sample13.js)]

    con el código siguiente:

    [!code-javascript[Main](3-creating-a-consistent-look/samples/sample14.js)]

    Como alternativa, puede reemplazar la línea de código anterior con el siguiente bloque de código, que genera los mismos resultados:

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample15.cshtml)]
4. Ejecute el *Content2.cshtml* página en un explorador nuevo. (Si todavía tiene abierto en el Explorador de esta página, puede simplemente actualizarlo.) Esta vez la página se muestra con ningún error, incluso aunque no tenga ningún encabezado.

## <a name="passing-data-to-layout-pages"></a>Pasar datos a páginas de diseño

Es posible que tenga datos definidos en la página de contenido que necesita para hacer referencia en una página de diseño. Si es así, debe pasar los datos de la página de contenido a la página de diseño. Por ejemplo, desea mostrar el estado de inicio de sesión de un usuario, o desee mostrar u ocultar las áreas de contenido en función de los proporcionados por el usuario.

Para pasar datos de una página de contenido a una página de diseño, puede colocar valores en la `PageData` propiedades de la página de contenido. El `PageData` propiedad es una colección de pares de nombre/valor que contienen los datos que se van a pasar entre las páginas. En la página de diseño, a continuación, puede leer valores fuera de la `PageData` propiedad.

Este es otro diagrama. Este muestra cómo puede utilizar ASP.NET la `PageData` propiedad para pasar valores de una página de contenido a la página de diseño. Cuando ASP.NET inicia la compilación de la página web, crea el `PageData` colección. En la página de contenido, se escribe código para colocar datos en el `PageData` colección. Los valores en el `PageData` colección también puede obtenerse mediante otras secciones de la página de contenido o bloques de contenido adicionales.

![Diagrama conceptual que muestre cómo puede rellenar un diccionario PageData y pasar esa información a la página de diseño de una página de contenido.](3-creating-a-consistent-look/_static/image8.jpg)

El siguiente procedimiento muestra cómo pasar datos de una página de contenido a una página de diseño. Cuando se ejecuta la página, muestra un botón que permite al usuario mostrar u ocultar una lista que se define en la página de diseño. Cuando los usuarios haga clic en el botón, establece un valor de verdadero/falso (booleano) en el `PageData` propiedad. La página de diseño lee ese valor y si es false, oculta la lista. El valor también se usa en la página de contenido para determinar si se debe mostrar el **Ocultar lista** botón o **Mostrar lista** botón.

![[image]](3-creating-a-consistent-look/_static/image9.jpg)

1. En la carpeta raíz, cree un archivo denominado *Content3.cshtml* y reemplace el contenido existente con lo siguiente:

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample16.cshtml)]

    El código almacena dos conjuntos de datos en el `PageData` propiedad &#8212; el título de la página web y true o false para especificar si desea mostrar una lista.

    Tenga en cuenta que ASP.NET le permite colocar el marcado HTML en la página de forma condicional mediante un bloque de código. Por ejemplo, el `if/else` bloque en el cuerpo de la página determina qué forma para mostrar dependiendo de si `PageData["ShowList"]` está establecido en true.
2. En el *Shared* carpeta, cree un archivo denominado  *\_Layout3.cshtml* y reemplace el contenido existente con lo siguiente:

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample17.cshtml)]

    La página de diseño incluye una expresión en el `<title>` elemento que recibe el valor de título de la `PageData` propiedad. También usa el `ShowList` valor de la `PageData` propiedad para determinar si se muestra el bloque de contenido de lista.
3. En el *Shared* carpeta, cree un archivo denominado  *\_List.cshtml* y reemplace el contenido existente con lo siguiente:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample18.html)]
4. Ejecute el *Content3.cshtml* página en un explorador. Se abrirá la página con la lista visible en el lado izquierdo de la página y un **Ocultar lista** situado en la parte inferior.

    ![Captura de pantalla que muestra la página que incluye la lista y un botón denominado "List ocultar".](3-creating-a-consistent-look/_static/image10.jpg)
5. Haga clic en **Ocultar lista**. Desaparece de la lista y el botón cambia a **Mostrar lista**.

    ![Captura de pantalla que muestra la página que no incluya la lista y un botón denominado 'Mostrar lista'.](3-creating-a-consistent-look/_static/image11.jpg)
6. Haga clic en el **Mostrar lista** botón y la lista se muestra de nuevo.

## <a name="additional-resources"></a>Recursos adicionales


[Personalizar el comportamiento de todo el sitio de ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202906)
