---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/ui_and_navigation
title: "Interfaz de usuario y la navegación | Documentos de Microsoft"
author: Erikre
description: "Esta serie de tutoriales le enseñará los aspectos básicos de la creación de una aplicación de formularios Web Forms de ASP.NET mediante ASP.NET 4.5 y Microsoft Visual Studio Express 2013 para se..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 5c76891d-e515-4885-b576-76bd2c494efe
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/ui_and_navigation
msc.type: authoredcontent
ms.openlocfilehash: 7f1d8a1a473820a7c8da4c8086904cc41c86fd2a
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="ui-and-navigation"></a>Interfaz de usuario y la navegación
====================
Por [Erik Reitan](https://github.com/Erikre)

[Descargar el proyecto de ejemplo de Wingtip Toys (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) o [descargar libros electrónicos (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Esta serie de tutoriales le enseñará los aspectos básicos de la creación de una aplicación de formularios Web Forms de ASP.NET con ASP.NET 4.5 y Microsoft Visual Studio Express 2013 para Web. Un Visual Studio 2013 [proyecto con código fuente de C#](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) está disponible como acompañamiento de esta serie de tutoriales.


En este tutorial, modificará la interfaz de usuario de la aplicación Web predeterminada para admitir las características de la aplicación de cliente de almacenamiento de Wingtip Toys. Además, agregará simple y navegación enlazado a datos. Este tutorial se basa en el tutorial anterior, "Crear la capa de acceso a datos" y forma parte de la serie de tutoriales de Wingtip Toys.

## <a name="what-youll-learn"></a>Lo que aprenderá:

- Cómo cambiar la interfaz de usuario para admitir las características de la aplicación de cliente de almacenamiento de Wingtip Toys.
- Cómo configurar un elemento de HTML5 para incluir la navegación en páginas.
- Cómo crear un control controladas por datos para navegar a los datos de producto específico.
- Cómo mostrar los datos de una base de datos creada con Entity Framework Code First.

Formularios Web Forms ASP.NET permiten crear contenido dinámico para la aplicación Web. Cada página Web de ASP.NET se crea de forma similar a una página HTML Web estática (es decir, una página que no se incluye el procesamiento basado en servidor), pero la página Web de ASP.NET incluye elementos adicionales que ASP.NET reconoce y procesa para generar el código HTML cuando se ejecuta la página.

Con una página HTML estática (*.htm* o *.html* archivo), el servidor cumple un `Web` solicitud leyendo el archivo y enviarlo como-consiste en el explorador. En cambio, cuando un usuario solicita una página Web ASP.NET (*.aspx* archivo), la página se ejecuta como un programa en el servidor Web. Mientras se ejecuta la página, puede realizar cualquier tarea que requiera el sitio Web, incluido el calcular valores, al leer o escribir la información de la base de datos o una llamada a otros programas. Como su resultado, la página dinámicamente genera marcado (por ejemplo, elementos de HTML) y envía este resultado dinámico al explorador.

## <a name="modifying-the-ui"></a>Modificar la interfaz de usuario

Podrá continuar esta serie de tutoriales modificando la *Default.aspx* página. Va a modificar la interfaz de usuario que ya se establece mediante la plantilla predeterminada utilizada para crear la aplicación. El tipo de modificaciones que se llevará a cabo son típicos al crear cualquier aplicación de formularios Web Forms. Llevará a cabo si se cambia el título, reemplaza parte del contenido y quitar contenido innecesario predeterminado.

1. Abra o cambie a la *Default.aspx* página.
2. Si aparece la página en **diseño** ver, cambiar a **origen** vista.
3. En la parte superior de la página en el `@Page` cambio de directiva, la `Title` atribuir a "Welcome", tal como se muestra resaltado en amarillo abajo. 

    [!code-aspx[Main](ui_and_navigation/samples/sample1.aspx?highlight=1)]
4. También en el *Default.aspx* página, reemplace todo el contenido predeterminado contenido en el `<asp:Content>` etiquetar para que el marcado aparece como a continuación. 

    [!code-aspx[Main](ui_and_navigation/samples/sample2.aspx)]
5. Guardar el *Default.aspx* página seleccionando **guardar Default.aspx** desde el **archivo** menú.

 Resultante *Default.aspx* página aparecerá como sigue: 

[!code-aspx[Main](ui_and_navigation/samples/sample3.aspx)]

En el ejemplo, ha establecido la `Title` atributo de la `@Page` directiva. Cuando se muestra el código HTML en un explorador, el código de servidor `<%: Page.Title %>` se resuelve en el contenido de la `Title` atributo.

La página de ejemplo incluye los elementos básicos que constituyen una página Web ASP.NET. La página contiene texto estático, es posible que tenga en una página HTML, junto con los elementos que son específicos de ASP.NET. El contenido de la *Default.aspx* página se integrará con el contenido de la página maestra, que se explican más adelante en este tutorial.

### <a name="page-directive"></a>@PageDirectiva

Formularios Web Forms ASP.NET normalmente contienen directivas que le permiten especificar información de propiedades y la configuración de página para la página. Las directivas se utilizan en ASP.NET como instrucciones sobre cómo debe procesar la página, pero no se representan como parte del marcado que se envía al explorador.

La directiva de usa más frecuente es la `@Page` directiva, que permite especificar muchas opciones de configuración de la página, incluidos los siguientes:

1. El servidor de lenguaje de programación de código en la página, como C#.
2. Si la página es una página con código de servidor directamente en la página, que se llama a una página de archivo único, o si es una página con el código en un archivo de clase independiente, que se denomina una página de código subyacente.
3. Si la página tiene una página maestra asociada y, por tanto, debe tratarse como una página de contenido.
4. La depuración y las opciones de traza.

Si no incluye un `@Page` la directiva en la página, o si la directiva no incluye una configuración específica, una configuración se heredará de la *Web.config* archivo de configuración o de la *Machine.config* archivo de configuración. El *Machine.config* archivo proporciona configuración adicional a todas las aplicaciones que se ejecutan en un equipo.

> [!NOTE] 
> 
> El *Machine.config* también proporciona información detallada sobre todas las configuraciones posibles.


### <a name="web-server-controls"></a>Controles de servidor Web

En la mayoría de las aplicaciones de formularios Web Forms de ASP.NET, agregará controles que permiten al usuario interactuar con la página, como botones, cuadros de texto, listas y así sucesivamente. Estos controles de servidor Web son similares a los botones HTML y elementos de entrada. Sin embargo, se procesan en el servidor, lo que le permite utilizar código de servidor para establecer sus propiedades. Estos controles también provocan eventos que puede controlar en código del servidor.

Controles de servidor utilizan una sintaxis especial que ASP.NET reconoce cuando se ejecuta la página. El nombre de etiqueta para controles de servidor ASP.NET comienza con un `asp:` prefijo. Esto permite a reconocer y procesar estos controles de servidor ASP.NET. El prefijo podría ser diferente si el control no forma parte de .NET Framework. Además el `asp:` prefijo, controles de servidor ASP.NET también incluyen el `runat="server"` atributo y un `ID` que puede usar para hacer referencia al control de código de servidor.

Cuando se ejecuta la página, ASP.NET identifica los controles de servidor y ejecuta el código que está asociado a los controles. Muchos controles representan código HTML u otro tipo de marcado en la página cuando se muestre en un explorador.

### <a name="server-code"></a>Código de servidor

La mayoría de las aplicaciones de formularios Web Forms de ASP.NET incluyen código que se ejecuta en el servidor cuando se procesa la página. Como se mencionó anteriormente, el código de servidor puede utilizarse para realizar diversas acciones, como agregar datos a un control ListView. ASP.NET admite muchos idiomas para que se ejecute en el servidor, incluyendo C#, Visual Basic, J# y otros.

ASP.NET admite dos modelos para escribir código de servidor de una página Web. En el modelo de archivo único, el código de la página está en un elemento de secuencia de comandos que incluye la etiqueta de apertura del `runat="server"` atributo. Como alternativa, puede crear el código de la página en un archivo de clase independiente, que se conoce como el modelo de código subyacente. En este caso, la página de formularios Web Forms de ASP.NET generalmente no contiene ningún código de servidor. En su lugar, el `@Page` directiva incluye información que vincula la *.aspx* página a su archivo de código subyacente asociado.

El `CodeBehind` atributos contenidos en el `@Page` directiva especifica el nombre del archivo de clase independiente y el `Inherits` atributo especifica el nombre de la clase en el archivo de código subyacente que corresponde a la página.

### <a name="updating-the-master-page"></a>Actualizar la página maestra

En los formularios Web Forms de ASP.NET, las páginas maestras permiten crear un diseño coherente para las páginas en la aplicación. Una sola página maestra define la apariencia y el comportamiento estándar que desea que tengan todas las páginas (o un grupo de páginas) de la aplicación. A continuación, puede crear páginas de contenido individuales que contienen el contenido que desea mostrar, como se explicó anteriormente. Cuando los usuarios solicitan las páginas de contenido, ASP.NET los combina con la página maestra para tener una salida que combine el diseño de la página maestra con el contenido de la página de contenido.

El nuevo sitio necesita un logotipo único que desea mostrar en cada página. Para agregar este logotipo, puede modificar el código HTML en la página maestra.

1. En **el Explorador de soluciones**, busque y abra la **Site.Master** página.
2. Si la página está en **diseño** ver, cambiar a **origen** vista.
3. Actualizar la página maestra por **modificar o agregar** el marcado que se resalta en amarillo: 

    [!code-aspx[Main](ui_and_navigation/samples/sample4.aspx?highlight=9,49,76-81,87)]

Este código HTML mostrará la imagen con el nombre *archivo logo.jpg* desde el *imágenes* carpeta de la aplicación Web, que agregará más adelante. Cuando se muestra una página que usa la página maestra en un explorador, se mostrará el logotipo. Si un usuario hace clic en el logotipo, el usuario se le remitirá a la *Default.aspx* página. La etiqueta delimitadora de HTML `<a>` ajusta el control image de servidor y permite que la imagen que debe incluirse como parte del vínculo. El `href` para la etiqueta delimitadora especifica la raíz del atributo "`~/`" del sitio Web como la ubicación del vínculo. De forma predeterminada, el *Default.aspx* página se muestra cuando el usuario navega a la raíz del sitio Web. El **imagen** `<asp:Image>` control de servidor incluye propiedades de suma, como `BorderStyle`, que se representan como HTML cuando se muestran en un explorador.

### <a name="master-pages"></a>Páginas maestras

Una página maestra es un archivo ASP.NET con la extensión. master (por ejemplo, *Site.Master*) con un diseño predefinido que puede incluir texto estático, los elementos HTML y controles de servidor. La página maestra se identifica mediante una clase especial `@Master` directiva que reemplaza el `@Page` directiva que se usa para normal *.aspx* páginas.

Además el `@Master` directiva, la página maestra también contiene todos los elementos HTML de nivel superior de una página, como `html`, `head`, y `form`. Por ejemplo, en la página maestra que agregó anteriormente, use un elemento HTML `table` para el diseño, un `img` elemento para el logotipo de la compañía, texto estático y controles de servidor para controlar la pertenencia comunes para su sitio. Puede usar código HTML y los elementos ASP.NET como parte de la página maestra.

Además de texto estático y controles que va a aparecer en todas las páginas, la página maestra también incluye uno o varios **ContentPlaceHolder** controles. Estos controles placeholder definen las regiones donde se mostrará el contenido reemplazable. A su vez, el contenido reemplazable se define en páginas de contenido, como *Default.aspx*, usando la **contenido** control de servidor.

#### <a name="adding-image-files"></a>Agregar archivos de imagen

La imagen de logotipo que se hace referencia anteriormente, junto con todas las imágenes de producto, debe agregarse a la aplicación Web, por lo que puede verse cuando el proyecto se muestra en un explorador.

#### <a name="download-from-msdn-samples-site"></a>Descargar desde el sitio de muestras de MSDN:

[Introducción a ASP.NET 4.5 Web Forms y Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)

La descarga incluye los recursos en el *WingtipToys activos* carpeta que se utilizan para crear la aplicación de ejemplo.

1. Si aún no lo ha hecho, descargue los archivos de ejemplo comprimido mediante el vínculo anterior desde el sitio de muestras de MSDN.
2. Una vez descargado, abra el archivo .zip y copie el contenido en una carpeta local en su equipo.
3. Busque y abra la *WingtipToys activos* carpeta.
4. Debe arrastrar y colocar, copiar el *catálogo* carpeta de la carpeta local a la raíz del proyecto de aplicación Web en el **el Explorador de soluciones** de Visual Studio. 

    ![Interfaz de usuario y la navegación: copiar archivos](ui_and_navigation/_static/image1.png)
5. A continuación, cree una carpeta nueva denominada *imágenes* haciendo clic en el **WingtipToys** proyecto en **el Explorador de soluciones** y seleccionando **agregar**  - &gt; **Nueva carpeta**.
6. Copia la *archivo logo.jpg* de archivos desde el *WingtipToys activos* carpeta en **Explorador de archivos** a la *imágenes* carpeta de la aplicación Web proyecto en **el Explorador de soluciones** de Visual Studio.
7. Haga clic en el **mostrar todos los archivos** opción en la parte superior de **el Explorador de soluciones** para actualizar la lista de archivos si no ve los nuevos archivos.  
  
    **El Explorador de soluciones** ahora muestra los archivos de proyecto actualizado. 

    ![Interfaz de usuario y la navegación: el Explorador de soluciones](ui_and_navigation/_static/image2.png)

### <a name="adding-pages"></a>Adición de páginas

Antes de agregar la navegación a la aplicación Web, agregará dos páginas nuevas que se desplazará a primero. Más adelante en esta serie de tutoriales, podrá mostrar productos y sus detalles en estas páginas nuevas.

1. En **el Explorador de soluciones**, haga clic en **WingtipToys**, haga clic en **agregar**y, a continuación, haga clic en **nuevo elemento**.   
 Se abrirá el cuadro de diálogo **Agregar nuevo elemento**.
2. Seleccione el **Visual C#**  - &gt; **Web** grupo de plantillas de la izquierda. A continuación, seleccione **formulario Web con página maestra** desde la mitad de lista y asígnele el nombre *ProductList.aspx*. 

    ![Interfaz de usuario y la navegación - Agregar nuevo elemento de cuadro de diálogo](ui_and_navigation/_static/image3.png)
3. Seleccione **Site.Master** para adjuntar la página maestra a recién creado *.aspx* página. 

    ![Interfaz de usuario y la navegación - Seleccionar página maestra](ui_and_navigation/_static/image4.png)
4. Agregar una página adicional denominada *ProductDetails.aspx* siguiendo estos mismos pasos.

### <a name="updating-bootstrap"></a>Arranque de actualización

Usan las plantillas de proyecto de Visual Studio 2013 [arranque](http://getbootstrap.com/), un marco de trabajo de diseño y creación de temas creado por Twitter. Bootstrap usa CSS3 para proporcionar un diseño dinámico, lo que significa diseños dinámicamente pueden adaptarse a los tamaños de ventana de explorador diferente. También puede utilizar la característica de temas del arranque para llevar a cabo fácilmente un cambio en la apariencia y funcionamiento de la aplicación. De forma predeterminada, la plantilla de aplicación Web ASP.NET en Visual Studio 2013 incluye Bootstrap como un paquete de NuGet.

En este tutorial, cambiará la apariencia y comportamiento de la aplicación Wingtip Toys reemplazando los archivos CSS de arranque.

1. En **el Explorador de soluciones**, abra el *contenido* carpeta.
2. Haga clic en el *bootstrap.css* archivo y cambie su nombre a *original.css de arranque*.
3. Cambiar el nombre de la *bootstrap.min.css* a *original.min.css de arranque*.
4. En **el Explorador de soluciones**, haga clic en el *contenido* carpeta y seleccione **Abrir carpeta en el Explorador de archivos**.  
 Se mostrará el Explorador de archivos. Aprenderá a guardar archivos CSS arranque descargado en esta ubicación.
5. En el explorador, vaya a [http://Bootswatch.com](http://bootswatch.com/).
6. Desplácese a la ventana del explorador hasta que vea el tema Cerulean. 

    ![Interfaz de usuario y la navegación - tema Cerulean](ui_and_navigation/_static/image5.png)
7. Descargar tanto la *bootstrap.css* archivo y la *bootstrap.min.css* del archivo a la *contenido* carpeta. Utilice la ruta de acceso a la carpeta de contenido que se muestra en el **Explorador de archivos** ventana que abrió anteriormente.
8. En **Visual Studio** en la parte superior de **el Explorador de soluciones**, seleccione la **mostrar todos los archivos** opción para mostrar los nuevos archivos en la carpeta de contenido. 

    ![Interfaz de usuario y la navegación: el Explorador de soluciones](ui_and_navigation/_static/image6.png)

 Verá los dos nuevos archivos CSS en el **contenido** carpeta, pero tenga en cuenta que el icono junto a cada nombre de archivo está deshabilitado. Esto significa que el archivo no se ha agregado al proyecto.
9. Haga clic en el *bootstrap.css* y *bootstrap.min.css* archivos y seleccione **incluir en el proyecto**.   
 Al ejecutar la aplicación de Wingtip Toys más adelante en este tutorial, se mostrará la nueva interfaz de usuario.

> [!NOTE] 
> 
> La plantilla de aplicación Web ASP.NET utiliza el *Bundle.config* archivo en la raíz del proyecto para almacenar la ruta de acceso de los archivos CSS de arranque.


### <a name="modifying-the-default-navigation"></a>Modificar la navegación predeterminada

El panel de navegación de forma predeterminada para todas las páginas en la aplicación se puede modificar cambiando el elemento de lista sin ordenar de navegación que se encuentra en la *Site.Master* página.

1. En **el Explorador de soluciones**, busque y abra la *Site.Master* página.
2. Agregar el vínculo de navegación adicionales aparecen resaltado en amarillo en la lista sin ordenar que se muestra a continuación:   

    [!code-html[Main](ui_and_navigation/samples/sample5.html?highlight=5)]

Como puede ver en el código HTML anterior, modificó cada artículo de línea `<li>` que contiene una etiqueta delimitadora `<a>` con un vínculo `href` atributo. Cada `href` apunta a una página de la aplicación Web. En el explorador, cuando un usuario hace clic en uno de estos vínculos (como **productos**), se le remitirá a la página contenida en el `href` (como **ProductList.aspx**). Se ejecutará la aplicación al final de este tutorial.

> [!NOTE] 
> 
> La tilde (`~`) carácter se usa para especificar que el `href` ruta de acceso comienza en la raíz del proyecto.


### <a name="adding-a-data-control-to-display-navigation-data"></a>Agregar un Control de datos para mostrar los datos de navegación

A continuación, agregará un control para mostrar todas las categorías de la base de datos. Cada categoría actuará como un vínculo a la *ProductList.aspx* página. Cuando un usuario hace clic en un vínculo de categoría en el explorador, que se vaya a la página de productos y ver solo los productos asociados a la categoría seleccionada.

Usará un **ListView** control para mostrar todas las categorías de la base de datos. Para agregar una **ListView** control a la página maestra:

1. En el *Site.Master* página, agregue las siguientes resaltadas `<div>` elemento **después** el `<div>` elemento que contiene el `id="TitleContent"` que agregó anteriormente:  

    [!code-aspx[Main](ui_and_navigation/samples/sample6.aspx?highlight=7-21)]

Este código mostrará todas las categorías de la base de datos. El **ListView** control muestra cada nombre de categoría como texto del vínculo e incluye un vínculo a la *ProductList.aspx* página con un valor de cadena de consulta que contenga el `ID` de la categoría. Estableciendo la `ItemType` propiedad en el **ListView** controlar, la expresión de enlace de datos `Item` está disponible en la `ItemTemplate` nodo y el control pasa a ser establecimiento inflexible de tipos. Puede seleccionar los detalles de la `Item` objeto con IntelliSense, como especificar el `CategoryName`. Este código se encuentra dentro del contenedor `<%#: %>` que marca una expresión de enlace de datos. Agregando (:) al final de la `<%#` prefijo, el resultado de la expresión de enlace de datos está codificado en HTML. Cuando el resultado está codificado en HTML, la aplicación está mejor protegida contra entre sitios inyección (XSS) y ataques de inyección de código HTML de script.

> [!NOTE] 
> 
> **Tip**
> 
> Cuando se agrega código escribiendo durante el desarrollo, puede estar seguro de que un miembro válido de un objeto se encuentra ya fuertemente tipadas controles de datos muestran a los miembros disponibles en función de IntelliSense. IntelliSense ofrece opciones de código adecuadas al contexto mientras escribe código, como propiedades, métodos y objetos.


En el siguiente paso, implementará la `GetCategories` método para recuperar los datos.

### <a name="linking-the-data-control-to-the-database"></a>Vincular el Control de datos a la base de datos

Antes de poder mostrar datos en el control de datos, debe vincular el control de datos a la base de datos. Para hacer que el vínculo, puede modificar el código subyacente de la *Site.Master.cs* archivo.

1. En **el Explorador de soluciones**, haga clic en el *Site.Master* página y, a continuación, haga clic en **ver código**. El *Site.Master.cs* archivo se abre en el editor.
2. Un nivel próximo al principio de la *Site.Master.cs* , agregue dos espacios de nombres adicionales para que todos los espacios de nombres incluye aparezcan como sigue:  

    [!code-csharp[Main](ui_and_navigation/samples/sample7.cs?highlight=8-9)]
3. Agregar el resaltado `GetCategories` método después de la `Page_Load` controlador de eventos como se indica a continuación:  

    [!code-csharp[Main](ui_and_navigation/samples/sample8.cs?highlight=6-11)]

El código anterior se ejecuta cuando se carga cualquier página que usa la página maestra en el explorador. El `ListView` control (con nombre "categoryList") que agregó anteriormente en este tutorial usa el enlace de modelos para seleccionar los datos. En el marcado de la `ListView` control se configura el control `SelectMethod` propiedad a la `GetCategories` método, se muestra arriba. El `ListView` controlar las llamadas de la `GetCategories` ciclo de método en el momento adecuado en la vida de la página y se enlaza automáticamente los datos devueltos. Aprenderá más acerca de enlace de datos en el tutorial siguiente.

### <a name="running-the-application-and-creating-the-database"></a>Ejecutar la aplicación y la creación de la base de datos

Anteriormente en esta serie de tutoriales se crea una clase de inicializador (denominada "ProductDatabaseInitializer") y se especifica esta clase en el *global.asax.cs* archivo. Entity Framework generará la base de datos cuando se ejecuta la primera vez que la aplicación porque el `Application_Start` método contenido en el *global.asax.cs* archivo llamará a la clase de inicializador. La clase de inicializador usará las clases del modelo (`Category` y `Product`) que agregó anteriormente en esta serie de tutoriales para crear la base de datos.

1. En **el Explorador de soluciones**, haga clic en el *Default.aspx* página y seleccione **establecer como página principal**.
2. En, presione Visual Studio **F5**.   
 Tardará un poco de tiempo para configurar la durante esta primera ejecución.   
    ![Interfaz de usuario y la navegación - ventanas del explorador](ui_and_navigation/_static/image7.png)  
 Al ejecutar la aplicación, la aplicación se compilará y la base de datos denominada *wingtiptoys.mdf* se creará en el *aplicación\_datos* carpeta. En el explorador, verá un menú de navegación de la categoría. Este menú se generó al recuperar las categorías de la base de datos. En el siguiente tutorial, implementará el panel de navegación.
3. Cierre el explorador para detener la aplicación en ejecución.

### <a name="reviewing-the-database"></a>Revisar la base de datos

Abra la *Web.config* de archivos y busque en la sección de la cadena de conexión. Puede ver que el `AttachDbFilename` valor en la cadena de conexión apunta a la `DataDirectory` para el proyecto de aplicación Web. El valor `|DataDirectory|` es un valor reservado que representa el *aplicación\_datos* carpeta del proyecto. Esta carpeta es donde se encuentra la base de datos que se creó a partir de las clases de entidad.

[!code-xml[Main](ui_and_navigation/samples/sample9.xml)]

> [!NOTE] 
> 
> Si el *aplicación\_datos* carpeta no está visible o si la carpeta está vacía, seleccione la **actualizar** icono y, a continuación, el **mostrar todos los archivos** situado en la parte superior de la **El Explorador de soluciones** ventana. Al expandir el ancho de la **el Explorador de soluciones** windows pueden ser necesaria para mostrar todos los iconos disponibles.


Ahora puede inspeccionar los datos contenidos en el *wingtiptoys.mdf* archivo de base de datos mediante el uso de la **Explorador de servidores** ventana.

1. Expanda el *aplicación\_datos* carpeta. Si el *aplicación\_datos* carpeta no está visible, consulte la nota anterior.
2. Si el *wingtiptoys.mdf* archivo de base de datos no está visible, seleccione la **actualizar** icono y, a continuación, el **mostrar todos los archivos** situado en la parte superior de la **el Explorador de soluciones**  ventana.
3. Haga clic en el *wingtiptoys.mdf* archivo de base de datos y seleccione **abiertos**.  
    **Explorador de servidores** se muestra. 

    ![Interfaz de usuario y la navegación - Explorador de servidores](ui_and_navigation/_static/image8.png)
4. Expanda el *tablas* carpeta.
5. Haga clic en el **productos**de tabla y seleccione **mostrar datos de tabla**.  
 El **productos** se muestra la tabla. 

    ![Interfaz de usuario y la navegación: tabla de productos](ui_and_navigation/_static/image9.png)
6. Esta vista le permite ver y modificar los datos de la **productos** tabla manualmente.
7. Cerrar la **productos** ventana de la tabla.
8. En el **Explorador de servidores**, haga clic en el **productos** de tabla nuevo y seleccione **Abrir definición de tabla**.  
 Los datos de diseño para la **productos** se muestra la tabla. 

    ![Interfaz de usuario y la navegación: diseño de productos](ui_and_navigation/_static/image10.png)
9. En el **T-SQL** ficha verá la instrucción DDL de SQL que se usó para crear la tabla. También puede usar la interfaz de usuario en el **diseño** ficha para modificar el esquema.
10. En el **Explorador de servidores**, haga clic en **WingtipToys** la base de datos y seleccione **cerrar conexión**.   
 Separando la base de datos de Visual Studio, el esquema de base de datos podrá modificarse más adelante en esta serie de tutoriales.
11. Vuelva a **el Explorador de soluciones**seleccionando el **el Explorador de soluciones** en la parte inferior de la pestaña de la **Explorador de servidores** ventana.

## <a name="summary"></a>Resumen

En este tutorial de la serie ha agregado alguna interfaz de usuario básica, gráficos, páginas y la navegación. Además, se ejecutó la aplicación Web, que crea la base de datos de las clases de datos que agregó en el tutorial anterior. También ver el contenido de la *productos* tabla de la base de datos mediante la visualización de la base de datos directamente. En el siguiente tutorial, podrá mostrar los elementos de datos y los detalles de la base de datos.

## <a name="additional-resources"></a>Recursos adicionales

[Introducción a la programación de las páginas Web ASP.NET](https://msdn.microsoft.com/library/ms178125.aspx)   
[Información general de controles de servidor Web de ASP.NET](https://msdn.microsoft.com/library/zsyt68f1.aspx)   
[Tutorial CSS](http://www.w3schools.com/css/default.asp)

>[!div class="step-by-step"]
[Anterior](create_the_data_access_layer.md)
[Siguiente](display_data_items_and_details.md)
