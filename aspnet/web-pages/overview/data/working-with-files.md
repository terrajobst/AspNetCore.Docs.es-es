---
uid: web-pages/overview/data/working-with-files
title: Trabajar con archivos en un sitio ASP.NET Web Pages (Razor) | Documentos de Microsoft
author: tfitzmac
description: "En este capítulo se explica cómo leer, escribir, anexar, eliminar y cargar archivos."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: eee916e4-ba4c-439a-a24e-68df7d45a569
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/data/working-with-files
msc.type: authoredcontent
ms.openlocfilehash: 0f119f8fb4873e55292203f21a2efd8f26793ae4
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="working-with-files-in-an-aspnet-web-pages-razor-site"></a>Trabajar con archivos en un sitio de ASP.NET Web Pages (Razor)
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
> - Cómo leer un archivo y mostrar de él.
> - Cómo eliminar archivos de un sitio Web.
> - Cómo permitir a los usuarios cargar uno o varios archivos.
> 
> Se trata de la programación de características introducidas en el artículo de ASP.NET:
> 
> - La `File` objeto, que proporciona una manera de administrar los archivos.
> - El `FileUpload` auxiliar.
> - La `Path` objeto, que proporciona métodos que permiten manipular los nombres de archivo y ruta de acceso.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software que se usa en el tutorial
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - WebMatrix 2
>   
> 
> Este tutorial también funciona con 3 de WebMatrix.


<a id="Creating_a_Text_File"></a>
## <a name="creating-a-text-file-and-writing-data-to-it"></a>Crear un archivo de texto y escribir datos en él

Además de usar una base de datos de su sitio Web, puede trabajar con archivos. Por ejemplo, puede usar archivos de texto como una manera sencilla de almacenar datos para el sitio. (A veces se denomina un archivo de texto que se usa para almacenar los datos de un *archivo plano*.) Archivos de texto pueden ser en formatos diferentes, como *.txt*, *.xml*, o *.csv* (valores separados por coma).

Si desea almacenar los datos en un archivo de texto, puede usar el `File.WriteAllText` método para especificar el archivo que desea crear y los datos que se va a escribir en él. En este procedimiento, creará una página que contiene un formulario sencillo con tres `input` elementos (nombre, apellido y dirección de correo electrónico) y un **enviar** botón. Cuando el usuario envía el formulario, va a almacenar la entrada del usuario en un archivo de texto.

1. Cree una carpeta nueva denominada *aplicación\_datos*, si aún no existe.
2. En la raíz del sitio Web, cree un nuevo archivo denominado *UserData.cshtml*.
3. Reemplace el contenido existente con lo siguiente: 

    [!code-cshtml[Main](working-with-files/samples/sample1.cshtml)]

    El formato HTML, crea el formulario con los tres cuadros de texto. En el código, utilice la `IsPost` propiedad para determinar si se ha enviado la página antes de iniciar el procesamiento.

    La primera tarea consiste en obtener datos proporcionados por el usuario y asignar a variables. El código, a continuación, concatena los valores de las variables independientes en una sola delimitada por comas, que, a continuación, se almacena en una variable distinta. Observe que el separador de coma es una cadena incluida entre comillas (","), porque literalmente vas a incrustar una coma en la cadena de gran tamaño que va a crear. Al final de los datos que concatenan juntos, agregar `Environment.NewLine`. Esto agrega un salto de línea (un carácter de nueva línea). Lo que va a crear con todos los esta concatenación es una cadena que el siguiente aspecto:

    [!code-css[Main](working-with-files/samples/sample2.css)]

    (Con un salto de línea invisible al final).

    A continuación, cree una variable (`dataFile`) que contiene la ubicación y el nombre del archivo para almacenar los datos en. Configuración de la ubicación, requiere algunos un tratamiento especial. En sitios Web, es una práctica incorrecta para hacer referencia en el código a las rutas de acceso absolutas como *C:\Folder\File.txt* para los archivos en el servidor web. Si se mueve un sitio Web, una ruta de acceso absoluta serán incorrecto. Además, para un sitio hospedado (en contraposición a su propio equipo) normalmente no sabe cuál es la ruta de acceso correcta cuando se va a escribir el código.

    Pero a veces (por ejemplo, ahora, para escribir en un archivo) necesita una ruta de acceso completa. La solución consiste en utilizar el `MapPath` método de la `Server` objeto. Esto devuelve la ruta de acceso completa a su sitio Web. Para obtener la ruta de acceso para la raíz del sitio Web, es un usuario de la `~` operador (para represen el sitio virtual de raíz) para `MapPath`. (También puede pasar un nombre de subcarpeta, como *~/App\_datos /*, para obtener la ruta de acceso para esa subcarpeta.) A continuación, puede concatenar información adicional en cualquier método que el que se devuelve con el fin de crear una ruta de acceso completa. En este ejemplo, se agrega un nombre de archivo. (Puede obtener más información sobre cómo trabajar con rutas de acceso de archivos y carpetas en [Introducción a ASP.NET Web Pages de programación mediante la sintaxis Razor](https://go.microsoft.com/fwlink/?LinkId=195205#ID_WorkingWithFileAndFolderPaths).)

    El archivo se guarda en el *aplicación\_datos* carpeta. Esta carpeta es una carpeta especial en ASP.NET que se usa para almacenar archivos de datos, como se describe en [Introducción a trabajar con una base de datos en sitios de ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=195209).

    El `WriteAllText` método de la `File` objeto escribe los datos en el archivo. Este método toma dos parámetros: el nombre (ruta de acceso) del archivo para escribir en y los datos reales para escribir. Tenga en cuenta que el nombre del primer parámetro tiene un `@` caracteres como prefijo. Esto indica a ASP.NET que se va a proporcionar un literal de cadena textual y que caracteres como "/" no debe interpretarse de formas especiales. (Para obtener más información, consulte [Introducción a ASP.NET Web programación mediante la sintaxis Razor](https://go.microsoft.com/fwlink/?LinkId=195205#ID_WorkingWithFileAndFolderPaths).)

    > [!NOTE]
    > Para el código guardar los archivos en el *aplicación\_datos* carpeta, la aplicación necesita permisos de lectura-escritura para esa carpeta. En el equipo de desarrollo esto no es normalmente un problema. Sin embargo, cuando se publica un sitio al servidor de un proveedor de hospedaje web, deberá establecer explícitamente los permisos. Si ejecuta este código en el servidor de un proveedor de hospedaje y obtiene errores, póngase en contacto con el proveedor de hospedaje para obtener información sobre cómo se establecen estos permisos.

- Ejecute la página en un explorador. 

    ![](working-with-files/_static/image1.jpg)
- Escriba valores en los campos y, a continuación, haga clic en **enviar**.
- Cierre el explorador.
- Vuelva al proyecto y actualice la vista.
- Abra la *data.txt* archivo. Los datos presentados en el formulario están en el archivo. 

    ![[image]](working-with-files/_static/image2.jpg)
- Cerrar la *data.txt* archivo.

<a id="Appending_Data"></a>
## <a name="appending-data-to-an-existing-file"></a>Anexar datos a un archivo existente

En el ejemplo anterior, ha utilizado `WriteAllText` para crear un archivo de texto que tiene solo una parte de los datos en ella. Si vuelve a llamar al método y pasar el mismo nombre de archivo, el archivo existente se sobrescribe completamente. Sin embargo, después de crear un archivo a menudo desea agregar nuevos datos al final del archivo. Puede hacer que el uso de la `AppendAllText` método de la `File` objeto.

1. En el sitio Web, realice una copia de la *UserData.cshtml* de archivos y el nombre de la copia *UserDataMultiple.cshtml*.
2. Reemplace el bloque de código antes de la apertura `<!DOCTYPE html>` etiqueta con el bloque de código siguiente: 

    [!code-cshtml[Main](working-with-files/samples/sample3.cshtml)]

    Este código contiene un cambio del ejemplo anterior. En lugar de usar `WriteAllText`, usa `the AppendAllText` método. Los métodos son similares, salvo que `AppendAllText` agrega los datos al final del archivo. Al igual que con `WriteAllText`, `AppendAllText` crea el archivo si aún no existe.
3. Ejecute la página en un explorador.
4. Especifique valores para los campos y, a continuación, haga clic en **enviar**.
5. Agregar más datos y enviar el formulario de nuevo.
6. Volver al proyecto, haga clic en la carpeta del proyecto y, a continuación, haga clic en **actualizar**.
7. Abra la *data.txt* archivo. Ahora contiene los nuevos datos que acaba de escribir. 

    ![[image]](working-with-files/_static/image3.jpg)

<a id="Reading_and_Displaying_Data"></a>
## <a name="reading-and-displaying-data-from-a-file"></a>Leer y mostrar datos desde un archivo

Aunque no es necesario escribir datos en un archivo de texto, probablemente a veces debe leer los datos de uno. Para ello, puede volver a usar el `File` objeto. Puede usar el `File` objeto para leer cada línea individualmente (separados por saltos de línea) o para leer un elemento individual con independencia de cómo está separadas.

Este procedimiento muestra cómo leer y mostrar los datos que creó en el ejemplo anterior.

1. En la raíz del sitio Web, cree un nuevo archivo denominado *DisplayData.cshtml*.
2. Reemplace el contenido existente con lo siguiente: 

    [!code-cshtml[Main](working-with-files/samples/sample4.cshtml)]

    El código que se inicia al leer el archivo que creó en el ejemplo anterior en una variable denominada `userData`, mediante esta llamada al método:

    [!code-css[Main](working-with-files/samples/sample5.css)]

    El código para hacer esto es dentro de un `if` instrucción. Si desea leer un archivo, es una buena idea usar el `File.Exists` método para determinar primero si el archivo está disponible. El código también comprueba si el archivo está vacío.

    El cuerpo de la página contiene dos `foreach` bucles, uno anidado dentro del otro. El exterior `foreach` bucle Obtiene una línea a la vez desde el archivo de datos. En este caso, las líneas se definen mediante saltos de línea en el archivo &#8212; es decir, cada elemento de datos en su propia línea. El bucle exterior crea un nuevo elemento (`<li>` elemento) dentro de una lista ordenada (`<ol>` elemento).

    El bucle interno divide cada línea de datos en elementos (campos) mediante una coma como delimitador. (Según el ejemplo anterior, esto significa que cada línea contiene tres campos &#8212; el nombre, apellido y dirección de correo electrónico, separan por punto y coma). El bucle interno también crea un `<ul>` de elementos de lista y muestra una lista de todos los campos de la línea de datos.

    El código muestra cómo utilizar dos tipos de datos, una matriz y el `char` tipo de datos. La matriz es necesaria porque el `File.ReadAllLines` método devuelve los datos como una matriz. El `char` tipo de datos es necesario porque el `Split` método devuelve un `array` en el que cada elemento es del tipo `char`. (Para obtener información acerca de las matrices, vea [Introducción a ASP.NET Web programación mediante la sintaxis Razor](https://go.microsoft.com/fwlink/?LinkId=202890#ID_CollectionsAndObjects).)
3. Ejecute la página en un explorador. Se muestran los datos especificados para los ejemplos anteriores. 

    ![[image]](working-with-files/_static/image4.jpg)

> [!TIP] 
> 
> **Mostrar datos desde un archivo delimitado por comas de Microsoft Excel**
> 
> Puede usar Microsoft Excel para guardar los datos contenidos en una hoja de cálculo como un archivo delimitado por comas (*.csv* archivo). Al hacerlo, el archivo se guarda en texto sin formato, no está en formato de Excel. Cada fila de la hoja de cálculo está separado por un salto de línea en el archivo de texto, y cada elemento de datos se separa por una coma. Puede usar el código mostrado en el ejemplo anterior para leer un archivo delimitado por comas de Excel simplemente cambiando el nombre del archivo de datos en el código.


<a id="Deleting_Files"></a>
## <a name="deleting-files"></a>Eliminar archivos

Para eliminar archivos de su sitio Web, puede usar el `File.Delete` método. Este procedimiento muestra cómo permitir a los usuarios eliminar una imagen (*.jpg* archivo) desde un *imágenes* carpeta si conoce el nombre del archivo.

> [!NOTE] 
> 
> **Importante** en un sitio Web de producción, normalmente restringir quién se puede realizar cambios en los datos. Para obtener información acerca de cómo configurar la pertenencia y formas de autorizar a los usuarios para realizar tareas en el sitio, consulte [agregar seguridad y la pertenencia a un sitio de ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202904).


1. En el sitio Web, cree una subcarpeta denominada *imágenes*.
2. Copia uno o más *.jpg* de archivos en el *imágenes* carpeta.
3. En la raíz del sitio Web, cree un nuevo archivo denominado *FileDelete.cshtml*.
4. Reemplace el contenido existente con lo siguiente: 

    [!code-cshtml[Main](working-with-files/samples/sample6.cshtml)]

    Esta página contiene un formulario donde los usuarios pueden especificar el nombre de un archivo de imagen. No se especifica la *.jpg* extensión de nombre de archivo; restringiendo el nombre de archivo similar al siguiente, que ayuda impide a los usuarios eliminar archivos de forma arbitraria en su sitio.

    El código lee el nombre de archivo que el usuario ha entrado en y, a continuación, crea una ruta de acceso completa. Para crear la ruta de acceso, el código utiliza la ruta de acceso del sitio Web actual (devuelto por la `Server.MapPath` método), el *imágenes* ".jpg" como una cadena literal, nombre de la carpeta y el nombre que el usuario ha proporcionado.

    Para eliminar el archivo, el código llama el `File.Delete` método, pasando la ruta de acceso completa que acaba de crear. Al final del marcado, código muestra un mensaje de confirmación que se eliminó el archivo.
5. Ejecute la página en un explorador. 

    ![[image]](working-with-files/_static/image5.jpg)
6. Escriba el nombre del archivo que desea eliminar y, a continuación, haga clic en **enviar**. Si se eliminó el archivo, se muestra el nombre del archivo en la parte inferior de la página.

<a id="Letting_Users_Upload_a_File"></a>
## <a name="letting-users-upload-a-file"></a>Lo que permite a los usuarios cargar un archivo

El `FileUpload` auxiliar permite a los usuarios cargar archivos en su sitio Web. El procedimiento siguiente muestra cómo permitir a los usuarios cargar un único archivo.

1. Agregar ASP.NET Web Helpers Library a su sitio Web, como se describe en [aplicaciones auxiliares de instalación en un sitio de ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372), si no agregó anteriormente.
2. En el *aplicación\_datos* carpeta, cree un nuevo una carpeta y asígnele el nombre *UploadedFiles*.
3. En la raíz, cree un nuevo archivo denominado *FileUpload.cshtml*.
4. Reemplace el contenido existente en la página con lo siguiente: 

    [!code-cshtml[Main](working-with-files/samples/sample7.cshtml)]

    La parte del cuerpo de la página usa el `FileUpload` auxiliar para crear el cuadro de carga y los botones que probablemente esté familiarizado con:

    ![[image]](working-with-files/_static/image6.jpg)

    Las propiedades configuradas para la `FileUpload` auxiliar especificar que desea que una única caja cargar el archivo y que desea que el botón Enviar para leer **cargar**. (Podrá agregar más cuadros más adelante en el artículo).

    Cuando el usuario hace clic en **cargar**, el código en la parte superior de la página recibe el archivo y lo guarda. El `Request` objeto que se utiliza normalmente para obtener valores de campos de formulario también tiene un `Files` matriz que contiene el archivo (o archivos) que se han cargado. Puede obtener los archivos individuales de posiciones concretas en la matriz &#8212; Por ejemplo, para obtener el primer archivo cargado, obtendrá `Request.Files[0]`, para obtener el segundo archivo, obtendrá `Request.Files[1]`, y así sucesivamente. (Recuerde que en la programación, contando normalmente comienza en cero).

    Al capturar un archivo cargado, se coloca en una variable (en este caso, `uploadedFile`) para que pueda procesarlos. Para determinar el nombre del archivo cargado, obtendrá simplemente su `FileName` propiedad. Sin embargo, cuando el usuario carga un archivo, `FileName` contiene el nombre del usuario original, que incluye la ruta de acceso completa. Podría ser similar al siguiente:

    *C:\Users\Public\Sample.txt*

    No desea toda esa información de ruta de acceso, sin embargo, dado es la ruta de acceso en el equipo del usuario, no para el servidor. Su intención es el nombre de archivo real (*Sample.txt*). Puede quitar simplemente el archivo de una ruta de acceso mediante el uso de la `Path.GetFileName` método, similar al siguiente:

    [!code-csharp[Main](working-with-files/samples/sample8.cs)]

    La `Path` objeto es una utilidad que tiene una serie de métodos como esta, que puede usar para quitar las rutas de acceso, combinar trazados y así sucesivamente.

    Una vez que ha llegado el nombre del archivo cargado, puede generar una nueva ruta de acceso donde desea almacenar el archivo cargado en el sitio Web. En este caso, se combinan `Server.MapPath`, los nombres de carpeta (*aplicación\_datos/UploadedFiles*) y el nombre de archivo recién eliminada para crear una nueva ruta de acceso. A continuación, puede llamar el archivo cargado `SaveAs` método realmente guardar el archivo.
5. Ejecute la página en un explorador. 

    ![[image]](working-with-files/_static/image7.jpg)
6. Haga clic en **examinar** y, a continuación, seleccione un archivo para cargarlo. 

    ![[image]](working-with-files/_static/image8.jpg)

    El cuadro de texto junto a la **examinar** botón contendrá la ruta de acceso y ubicación del archivo.

    ![[image]](working-with-files/_static/image9.jpg)
7. Haga clic en **Cargar**.
8. En el sitio Web, haga clic en la carpeta del proyecto y, a continuación, haga clic en **actualizar**.
9. Abra la *UploadedFiles* carpeta. Es el archivo que ha cargado en la carpeta. 

    ![[image]](working-with-files/_static/image10.jpg)

<a id="Letting_Users_Upload_Multiple_Files"></a>
## <a name="letting-users-upload-multiple-files"></a>Permitir que los usuarios cargan varios archivos

En el ejemplo anterior, permiten a los usuarios cargar un archivo. Pero puede usar el `FileUpload` auxiliar para cargar más de un archivo a la vez. Esto es útil para escenarios como cargar fotografías, donde resulta tedioso cargar un archivo a la vez. (Puede leer sobre cómo cargar fotos en [trabajar con imágenes en un sitio de ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202897).) En este ejemplo se muestra cómo permitir a los usuarios cargar dos a la vez, aunque puede usar la misma técnica para cargar más de que.

1. Agregar ASP.NET Web Helpers Library a su sitio Web, como se describe en [aplicaciones auxiliares de instalación en un sitio de ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372), si no lo ha hecho ya.
2. Crear una nueva página denominada *FileUploadMultiple.cshtml*.
3. Reemplace el contenido existente en la página con lo siguiente:  

    [!code-cshtml[Main](working-with-files/samples/sample9.cshtml)]

    En este ejemplo, el `FileUpload` auxiliar en el cuerpo de la página está configurado para permitir a los usuarios cargar dos archivos de forma predeterminada. Dado que `allowMoreFilesToBeAdded` se establece en `true`, la aplicación auxiliar representa un vínculo que permite el usuario agregar más cuadros de carga:

    ![[image]](working-with-files/_static/image11.jpg)

    Para procesar los archivos que el usuario carga, el código utiliza la misma técnica básica que utilizó en el ejemplo anterior &#8212; obtener un archivo de `Request.Files` y, a continuación, guárdelo. (Incluidos los varios elementos debe hacer para obtener el nombre de archivo correcto y la ruta.) La innovación este tiempo es que el usuario puede cargar varios archivos y no sé muchos. Para obtener información, puede obtener `Request.Files.Count`.

    Con este número de la mano, puede recorrer `Request.Files`, capture a su vez cada archivo y guárdelo. Si desea crear un bucle un número determinado de veces a través de una colección, puede usar un `for` bucles, similar al siguiente:

    [!code-csharp[Main](working-with-files/samples/sample10.cs)]

    La variable `i` es simplemente un contador temporal que irá desde cero hasta el límite superior que haya establecido. En este caso, el límite superior es el número de archivos. Pero, dado que el contador comienza en cero, tal y como es habitual para el recuento de escenarios en ASP.NET, el límite superior es realmente una menor que el número de archivos. (Si se cargan los tres archivos, el recuento es cero a 2).

    El `uploadedCount` variable totales de todos los archivos que se cargan y se guardó correctamente. Este código representa la posibilidad de que un archivo esperado no pueda cargarse.
4. Ejecute la página en un explorador. El explorador muestra la página y sus cuadros de carga de dos.
5. Seleccione dos archivos para cargar.
6. Haga clic en **agregar otro archivo**. La página muestra un cuadro de la carga de nuevo. 

    ![[image]](working-with-files/_static/image12.jpg)
7. Haga clic en **Cargar**.
8. En el sitio Web, haga clic en la carpeta del proyecto y, a continuación, haga clic en **actualizar**.
9. Abra la *UploadedFiles* carpeta para ver los archivos cargados correctamente.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Recursos adicionales


[Trabajar con imágenes en un sitio de páginas Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202897)

[Exportar a un archivo CSV](https://msdn.microsoft.com/library/ms155919.aspx)
