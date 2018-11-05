---
uid: web-pages/overview/data/working-with-files
title: Trabajar con archivos en un sitio ASP.NET Web Pages (Razor) | Microsoft Docs
author: Rick-Anderson
description: Este capítulo explica cómo leer, escribir, anexar, eliminar y cargar archivos.
ms.author: riande
ms.date: 02/20/2014
ms.assetid: eee916e4-ba4c-439a-a24e-68df7d45a569
msc.legacyurl: /web-pages/overview/data/working-with-files
msc.type: authoredcontent
ms.openlocfilehash: 8b9176cb88f6e460fe5494167d4a5880456530aa
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021565"
---
<a name="working-with-files-in-an-aspnet-web-pages-razor-site"></a>Trabajar con archivos en un sitio Web de ASP.NET Pages (Razor)
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> En este artículo se explica cómo leer, escribir, anexar, eliminar y cargar archivos en un sitio de ASP.NET Web Pages (Razor).
> 
> > [!NOTE]
> > Si desea cargar imágenes y manipularlos (por ejemplo, voltear o cambiar su tamaño), consulte [trabajar con imágenes en un sitio de ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202897).
> 
> 
> **Lo que aprenderá:** 
> 
> - Cómo crear un archivo de texto y escribir datos en él.
> - Cómo anexar datos a un archivo existente.
> - Cómo leer un archivo y mostrar a partir de él.
> - Cómo eliminar archivos desde un sitio Web.
> - Cómo permitir a los usuarios cargar uno o varios archivos.
> 
> Estas son las características introducidas en el artículo de programación de ASP.NET:
> 
> - El `File` object, que proporciona una manera de administrar los archivos.
> - El `FileUpload` auxiliar.
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


<a id="Creating_a_Text_File"></a>
## <a name="creating-a-text-file-and-writing-data-to-it"></a>Creación de un archivo de texto y escribir datos en él

Además de usar una base de datos en su sitio Web, puede trabajar con archivos. Por ejemplo, puede usar archivos de texto como una manera sencilla para almacenar los datos del sitio. (A veces se llama a un archivo de texto que se usa para almacenar los datos de un *archivos planos*.) Archivos de texto pueden ser en diferentes formatos, como *.txt*, *.xml*, o *.csv* (valores separados por coma).

Si desea almacenar los datos en un archivo de texto, puede usar el `File.WriteAllText` método para especificar el archivo para crear y los datos que se va a escribir en él. En este procedimiento, creará una página que contiene un formulario simple con tres `input` elementos (nombre, apellido y dirección de correo electrónico) y un **enviar** botón. Cuando el usuario envía el formulario, que almacenará la entrada del usuario en un archivo de texto.

1. Cree una carpeta nueva denominada *aplicación\_datos*, si aún no existe.
2. En la raíz de su sitio Web, cree un nuevo archivo denominado *UserData.cshtml*.
3. Reemplace el contenido existente por lo siguiente: 

    [!code-cshtml[Main](working-with-files/samples/sample1.cshtml)]

    El marcado HTML crea el formulario con los tres cuadros de texto. En el código, usa el `IsPost` propiedad para determinar si se ha enviado la página antes de iniciar el procesamiento.

    La primera tarea consiste en obtener la entrada del usuario y asignarlo a las variables. El código, a continuación, concatena los valores de las variables independientes en una sola delimitada por comas, que, a continuación, se almacena en otra variable. Tenga en cuenta que el separador de coma es una cadena incluida entre comillas (","), porque lo está insertando literalmente una coma en la cadena de gran tamaño que se va a crear. Al final de los datos que concatenan juntos, agrega `Environment.NewLine`. Esto agrega un salto de línea (un carácter de nueva línea). Lo que está creando con toda esta concatenación es una cadena que tiene este aspecto:

    [!code-css[Main](working-with-files/samples/sample2.css)]

    (Con un salto de línea invisible al final).

    A continuación, cree una variable (`dataFile`) que contiene la ubicación y el nombre del archivo para almacenar los datos. La ubicación de la configuración requiere algún tratamiento especial. En los sitios Web, es una práctica incorrecta para hacer referencia en el código a las rutas de acceso absolutas como *C:\Folder\File.txt* para los archivos en el servidor web. Si se mueve un sitio Web, una ruta de acceso absoluta será incorrecta. Además, para un sitio hospedado (en contraposición a su propio equipo) normalmente ni siquiera sabe lo que es la ruta de acceso correcta al escribir el código.

    Pero a veces (por ejemplo, ahora, para escribir en un archivo) necesita una ruta de acceso completa. La solución consiste en usar el `MapPath` método de la `Server` objeto. Esto devuelve la ruta de acceso completa a su sitio Web. Para obtener la ruta de acceso para la raíz del sitio Web, es un usuario de la `~` operador (para represen el sitio de virtual raíz) para `MapPath`. (También puede pasar un nombre de la subcarpeta, como *~/App\_datos /*, para obtener la ruta de acceso para esa subcarpeta.) A continuación, puede concatenar información adicional en cualquier método la devuelve con el fin de crear una ruta de acceso completa. En este ejemplo, agregue un nombre de archivo. (Puede leer más acerca de cómo trabajar con rutas de acceso de archivos y carpetas en [Introducción a ASP.NET Web Pages de programación mediante la sintaxis Razor](https://go.microsoft.com/fwlink/?LinkId=195205#ID_WorkingWithFileAndFolderPaths).)

    El archivo se guarda en el *aplicación\_datos* carpeta. Esta carpeta es una carpeta especial en ASP.NET que se usa para almacenar archivos de datos, como se describe en [Introducción al trabajo con una base de datos en sitios de ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=195209).

    El `WriteAllText` método de la `File` objeto escribe los datos en el archivo. Este método toma dos parámetros: el nombre (con la ruta de acceso) del archivo para escribir en y los datos reales para escribir. Tenga en cuenta que el nombre del primer parámetro tiene un `@` carácter como prefijo. Esto indica a ASP.NET que está proporcionando un literal de cadena textual y caracteres como "/" no debe interpretarse de formas especiales. (Para obtener más información, consulte [Introducción a ASP.NET Web programación mediante la sintaxis Razor](https://go.microsoft.com/fwlink/?LinkId=195205#ID_WorkingWithFileAndFolderPaths).)

    > [!NOTE]
    > Para el código guardar los archivos en el *aplicación\_datos* carpeta, la aplicación necesita permisos de lectura-escritura para esa carpeta. En el equipo de desarrollo esto no es normalmente un problema. Sin embargo, cuando se publica un sitio al servidor de un proveedor de hospedaje web, debe establecer explícitamente esos permisos. Si ejecuta este código en el servidor de un proveedor de hospedaje y obtiene errores, póngase en contacto con el proveedor de hospedaje para obtener información sobre cómo establecer estos permisos.

- Ejecute la página en un explorador. 

    ![](working-with-files/_static/image1.jpg)
- Escriba valores en los campos y, a continuación, haga clic en **enviar**.
- Cierre el explorador.
- Vuelva al proyecto y actualice la vista.
- Abra el *data.txt* archivo. Los datos que ha enviado en el formulario están en el archivo. 

    ![[image]](working-with-files/_static/image2.jpg)
- Cerrar la *data.txt* archivo.

<a id="Appending_Data"></a>
## <a name="appending-data-to-an-existing-file"></a>Anexar datos a un archivo existente

En el ejemplo anterior, ha utilizado `WriteAllText` para crear un archivo de texto que tiene un solo elemento de datos en ella. Si vuelve a llamar al método y pásele el nombre de archivo mismo, el archivo existente se sobrescribe completamente. Sin embargo, después de crear un archivo a menudo desea agregar datos nuevos al final del archivo. Puede hacerlo mediante el `AppendAllText` método de la `File` objeto.

1. En el sitio Web, realice una copia de la *UserData.cshtml* de archivos y el nombre de *UserDataMultiple.cshtml*.
2. Reemplace el bloque de código antes de la apertura `<!DOCTYPE html>` etiqueta con el bloque de código siguiente: 

    [!code-cshtml[Main](working-with-files/samples/sample3.cshtml)]

    Este código tiene un cambio en el ejemplo anterior. En lugar de usar `WriteAllText`, usa `the AppendAllText` método. Los métodos son similares, salvo que `AppendAllText` agrega los datos al final del archivo. Igual que con `WriteAllText`, `AppendAllText` crea el archivo si aún no existe.
3. Ejecute la página en un explorador.
4. Especifique valores para los campos y, a continuación, haga clic en **enviar**.
5. Agregar más datos y envía el formulario de nuevo.
6. Volver a su proyecto, haga clic en la carpeta del proyecto y, a continuación, haga clic en **actualizar**.
7. Abra el *data.txt* archivo. Ahora contiene los nuevos datos que acaba de escribir. 

    ![[image]](working-with-files/_static/image3.jpg)

<a id="Reading_and_Displaying_Data"></a>
## <a name="reading-and-displaying-data-from-a-file"></a>Leer y mostrar datos desde un archivo

Incluso si no necesita escribir datos en un archivo de texto, probablemente a veces deba leer datos de uno. Para ello, puede volver a usar el `File` objeto. Puede usar el `File` objeto leer cada línea individualmente (separados por saltos de línea) o para leer un elemento individual con independencia de cómo se separan.

Este procedimiento muestra cómo leer y mostrar los datos que creó en el ejemplo anterior.

1. En la raíz de su sitio Web, cree un nuevo archivo denominado *DisplayData.cshtml*.
2. Reemplace el contenido existente por lo siguiente: 

    [!code-cshtml[Main](working-with-files/samples/sample4.cshtml)]

    El código comienza por leer el archivo que creó en el ejemplo anterior en una variable denominada `userData`, mediante esta llamada al método:

    [!code-css[Main](working-with-files/samples/sample5.css)]

    El código para hacer esto es dentro de un `if` instrucción. Cuando desea leer un archivo, es una buena idea usar el `File.Exists` método para determinar primero si el archivo está disponible. El código también comprueba si el archivo está vacío.

    El cuerpo de la página contiene dos `foreach` bucles, uno anidado dentro de otro. El exterior `foreach` bucle Obtiene una línea a la vez desde el archivo de datos. En este caso, las líneas se definen mediante saltos de línea en el archivo &#8212; es decir, cada elemento de datos en su propia línea. El bucle exterior crea un nuevo elemento (`<li>` elemento) dentro de una lista ordenada (`<ol>` elemento).

    El bucle interior divide cada línea de datos en los elementos (campos) con una coma como delimitador. (Según el ejemplo anterior, esto significa que cada línea contiene tres campos &#8212; el nombre, apellido y dirección de correo electrónico, separados por una coma.) El bucle interno crea también un `<ul>` lista y muestra una lista de elementos para cada campo de la línea de datos.

    El código muestra cómo usar dos tipos de datos, una matriz y el `char` tipo de datos. La matriz es necesaria porque el `File.ReadAllLines` método devuelve los datos como una matriz. El `char` tipo de datos es necesario porque el `Split` método devuelve un `array` en el que cada elemento es del tipo `char`. (Para obtener información acerca de las matrices, vea [Introducción a ASP.NET Web programación mediante la sintaxis Razor](https://go.microsoft.com/fwlink/?LinkId=202890#ID_CollectionsAndObjects).)
3. Ejecute la página en un explorador. Se muestran los datos especificados en los ejemplos anteriores. 

    ![[image]](working-with-files/_static/image4.jpg)

> [!TIP] 
> 
> **Mostrar datos desde un archivo delimitado por comas de Microsoft Excel**
> 
> Puede usar Microsoft Excel para guardar los datos contenidos en una hoja de cálculo como un archivo delimitado por comas (*.csv* archivo). Al hacerlo, se guarda el archivo de texto sin formato, no en el formato de Excel. Cada fila de la hoja de cálculo está separado por un salto de línea del archivo de texto, y cada elemento de datos está separado por una coma. Puede usar el código que se muestra en el ejemplo anterior para leer un archivo delimitado por comas de Excel cambiando el nombre del archivo de datos en el código.


<a id="Deleting_Files"></a>
## <a name="deleting-files"></a>Eliminación de archivos

Para eliminar archivos desde su sitio Web, puede usar el `File.Delete` método. Este procedimiento muestra cómo permitir que los usuarios eliminar una imagen (*.jpg* archivo) desde un *imágenes* carpeta si conoce el nombre del archivo.

> [!NOTE] 
> 
> **Importante** en un sitio Web de producción, normalmente restringe quién se puede realizar cambios en los datos. Para obtener información acerca de cómo configurar la pertenencia y formas de autorizar a los usuarios realizar tareas en el sitio, consulte [agregar seguridad y la pertenencia a un sitio de ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202904).


1. En el sitio Web, cree una subcarpeta denominada *imágenes*.
2. Copia uno o más *.jpg* archivos en el *imágenes* carpeta.
3. En la raíz del sitio Web, cree un nuevo archivo denominado *FileDelete.cshtml*.
4. Reemplace el contenido existente por lo siguiente: 

    [!code-cshtml[Main](working-with-files/samples/sample6.cshtml)]

    Esta página contiene un formulario donde los usuarios pueden escribir el nombre de un archivo de imagen. Que no entre en el *.jpg* extensión de nombre de archivo; restringiendo el nombre de archivo como esta, que ayuda impide que los usuarios elimine archivos arbitrarios de su sitio.

    El código lee el nombre de archivo que el usuario ha escrito y, a continuación, crea una ruta de acceso completa. Para crear la ruta de acceso, el código usa la ruta de acceso del sitio Web actual (devuelto por la `Server.MapPath` método), el *imágenes* ".jpg" como una cadena literal, el nombre que el usuario ha proporcionado y nombre de la carpeta.

    Para eliminar el archivo, el código llama a la `File.Delete` método y pásele la ruta de acceso completa que acaba de crear. Al final del marcado, código muestra un mensaje de confirmación que se ha eliminado el archivo.
5. Ejecute la página en un explorador. 

    ![[image]](working-with-files/_static/image5.jpg)
6. Escriba el nombre del archivo que desea eliminar y, a continuación, haga clic en **enviar**. Si se eliminó el archivo, el nombre del archivo se muestra en la parte inferior de la página.

<a id="Letting_Users_Upload_a_File"></a>
## <a name="letting-users-upload-a-file"></a>Lo que permite a los usuarios cargar un archivo

El `FileUpload` auxiliar permite a los usuarios cargar archivos en su sitio Web. El procedimiento siguiente muestra cómo permitir que los usuarios cargar un único archivo.

1. Agregue la ASP.NET Web Helpers Library a su sitio Web, como se describe en [las aplicaciones auxiliares de la instalación en un sitio de ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372), si no agregó anteriormente.
2. En el *aplicación\_datos* carpeta, cree un nuevo de una carpeta y asígnele el nombre *UploadedFiles*.
3. En la raíz, cree un nuevo archivo denominado *FileUpload.cshtml*.
4. Reemplace el contenido existente en la página con lo siguiente: 

    [!code-cshtml[Main](working-with-files/samples/sample7.cshtml)]

    La parte del cuerpo de la página usa el `FileUpload` auxiliar para crear el cuadro de carga y los botones que le resulte familiar:

    ![[image]](working-with-files/_static/image6.jpg)

    Las propiedades configuradas para el `FileUpload` auxiliar especificar que desea que un cuadro único para el archivo para cargar y que desea que el botón Enviar para leer **cargar**. (Agregará más cuadros más adelante en este artículo.)

    Cuando el usuario hace clic en **cargar**, el código en la parte superior de la página obtiene el archivo y lo guarda. El `Request` objeto que utiliza normalmente para obtener valores de los campos de formulario también tiene un `Files` matriz que contiene el archivo (o archivos) que se han cargado. Puede obtener los archivos individuales fuera de las posiciones específicas de la matriz &#8212; por ejemplo, para obtener el primer archivo cargado, obtener `Request.Files[0]`, para obtener el segundo archivo, obtendrá `Request.Files[1]`, y así sucesivamente. (Recuerde que en la programación, contando normalmente comienza en cero).

    Al recuperar un archivo cargado, se coloca en una variable (en este caso, `uploadedFile`) para que pueda procesarlos. Para determinar el nombre del archivo cargado, sólo obtendrá su `FileName` propiedad. Sin embargo, cuando el usuario carga un archivo, `FileName` contiene el nombre del usuario original, que incluye la ruta de acceso completa. Podría parecerse a esto:

    *C:\Users\Public\Sample.txt*

    Sin embargo, no desea toda esa información de ruta de acceso, ya que es la ruta de acceso en el equipo del usuario, no para el servidor. Tan solo quiere el nombre de archivo real (*Sample.txt*). Puede eliminar solo el archivo desde una ruta de acceso mediante el uso de la `Path.GetFileName` método, similar al siguiente:

    [!code-csharp[Main](working-with-files/samples/sample8.cs)]

    La `Path` objeto es una utilidad que tiene una serie de métodos, así que puede usar para quitar rutas de acceso, combinar trazados y así sucesivamente.

    Una vez que ha llegado el nombre del archivo cargado, puede crear una nueva ruta de acceso donde desea almacenar el archivo cargado en su sitio Web. En este caso, combinan `Server.MapPath`, los nombres de carpeta (*aplicación\_datos/UploadedFiles*) y el nombre de archivo recién eliminados para crear una nueva ruta de acceso. A continuación, puede llamar el archivo cargado `SaveAs` método realmente guardar el archivo.
5. Ejecute la página en un explorador. 

    ![[image]](working-with-files/_static/image7.jpg)
6. Haga clic en **examinar** y, a continuación, seleccione un archivo para cargarlo. 

    ![[image]](working-with-files/_static/image8.jpg)

    El cuadro de texto junto a la **examinar** botón contendrá la ruta de acceso y ubicación del archivo.

    ![[image]](working-with-files/_static/image9.jpg)
7. Haga clic en **Cargar**.
8. En el sitio Web, haga clic en la carpeta del proyecto y, a continuación, haga clic en **actualizar**.
9. Abra el *UploadedFiles* carpeta. El archivo cargado está en la carpeta. 

    ![[image]](working-with-files/_static/image10.jpg)

<a id="Letting_Users_Upload_Multiple_Files"></a>
## <a name="letting-users-upload-multiple-files"></a>Permitir que los usuarios cargar varios archivos

En el ejemplo anterior, permiten a los usuarios cargar un archivo. Pero puede usar el `FileUpload` auxiliar para cargar varios archivos a la vez. Esto es útil para escenarios como carga de fotos, donde cargar un archivo cada vez es una tarea tediosa. (Encontrará información sobre cómo cargar fotografías en [trabajar con imágenes en un sitio de ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202897).) Este ejemplo muestra cómo permitir que los usuarios cargar dos a la vez, aunque puede usar la misma técnica para cargar más que eso.

1. Agregue la ASP.NET Web Helpers Library a su sitio Web, como se describe en [las aplicaciones auxiliares de la instalación en un sitio de ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372), si no lo ha hecho ya.
2. Crear una nueva página denominada *FileUploadMultiple.cshtml*.
3. Reemplace el contenido existente en la página con lo siguiente:  

    [!code-cshtml[Main](working-with-files/samples/sample9.cshtml)]

    En este ejemplo, el `FileUpload` auxiliar en el cuerpo de la página está configurado para permitir que los usuarios cargar dos archivos de forma predeterminada. Dado que `allowMoreFilesToBeAdded` está establecido en `true`, la aplicación auxiliar representa un vínculo que permite al usuario agregar más cuadros de carga:

    ![[image]](working-with-files/_static/image11.jpg)

    Para procesar los archivos que el usuario cargue, el código usa la misma técnica básica que usó en el ejemplo anterior &#8212; obtener un archivo de `Request.Files` y, a continuación, guárdelo. (Incluidas las diversas cosas debe hacer para obtener el nombre de archivo correcto y la ruta de acceso.) La innovación en este momento es que el usuario podría tener que cargar varios archivos y no sabe muchos. Para averiguar, puede obtener `Request.Files.Count`.

    Con este número en mano, puede recorrer `Request.Files`, capturar a su vez cada archivo y guárdelo. Cuando desee que se ejecute en bucle un número determinado de veces a través de una colección, puede usar un `for` bucle, similar al siguiente:

    [!code-csharp[Main](working-with-files/samples/sample10.cs)]

    La variable `i` es simplemente un contador temporal que irá desde cero hasta el límite superior establecido. En este caso, el límite superior es el número de archivos. Pero, dado que el contador comienza en cero, como es típico para el recuento de escenarios en ASP.NET, el límite superior es realmente una menor que el número de archivos. (Si se cargan los tres archivos, el recuento es cero a 2).

    El `uploadedCount` variable totales de todos los archivos que se cargan y se guardó correctamente. Este código en cuenta la posibilidad de que un archivo esperado no pueda cargarse.
4. Ejecute la página en un explorador. El explorador muestra la página y sus cuadros de carga de dos.
5. Seleccione dos archivos para cargar.
6. Haga clic en **agregar otro archivo**. La página muestra un cuadro de la carga de nuevo. 

    ![[image]](working-with-files/_static/image12.jpg)
7. Haga clic en **Cargar**.
8. En el sitio Web, haga clic en la carpeta del proyecto y, a continuación, haga clic en **actualizar**.
9. Abra el *UploadedFiles* carpeta para ver los archivos cargados correctamente.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Recursos adicionales


[Trabajar con imágenes en un sitio de ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202897)

[Exportar a un archivo CSV](https://msdn.microsoft.com/library/ms155919.aspx)
