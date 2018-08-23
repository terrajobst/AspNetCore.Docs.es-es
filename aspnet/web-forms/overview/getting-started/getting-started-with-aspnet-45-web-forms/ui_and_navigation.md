---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/ui_and_navigation
title: Interfaz de usuario y navegación | Microsoft Docs
author: Erikre
description: Esta serie de tutoriales aprenderá los conceptos básicos de la creación de una aplicación de formularios Web Forms ASP.NET con ASP.NET 4.5 y Microsoft Visual Studio Express 2013 para se...
ms.author: riande
ms.date: 09/08/2014
ms.assetid: 5c76891d-e515-4885-b576-76bd2c494efe
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/ui_and_navigation
msc.type: authoredcontent
ms.openlocfilehash: 55c659cbaf48dbb02dc34e013242443d4fbd8845
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837252"
---
<a name="ui-and-navigation"></a>Interfaz de usuario y navegación
====================
por [Erik Reitan](https://github.com/Erikre)

[Descargar el proyecto de ejemplo de Wingtip Toys (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) o [descargar eBook (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Esta serie de tutoriales aprenderá los conceptos básicos de la creación de una aplicación de formularios Web Forms ASP.NET con ASP.NET 4.5 y Microsoft Visual Studio Express 2013 para Web. Un Visual Studio 2013 [proyecto con código fuente de C#](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) está disponible para acompañar esta serie de tutoriales.


En este tutorial, modificará la interfaz de usuario de la aplicación Web predeterminada para admitir las características de la aplicación cliente de almacenamiento de Wingtip Toys. Además, se agregará simple y navegación enlazado a datos. Este tutorial se basa en el tutorial anterior "Creación de la capa de acceso a datos" y forma parte de la serie de tutoriales de Wingtip Toys.

## <a name="what-youll-learn"></a>Lo que aprenderá:

- Cómo cambiar la interfaz de usuario para admitir las características de la aplicación cliente de almacenamiento de Wingtip Toys.
- Cómo configurar un elemento de HTML5 para incluir la navegación de páginas.
- Cómo crear un control controlado por datos para navegar a los datos de producto específico.
- Cómo mostrar los datos de una base de datos mediante Entity Framework Code First.

ASP.NET Web Forms permiten crear contenido dinámico para la aplicación Web. Cada página Web de ASP.NET se crea de forma similar a una página HTML Web estática (es decir, una página que no se incluye el procesamiento basado en servidor), pero la página Web ASP.NET incluye elementos adicionales que ASP.NET reconoce y procesa para generar código HTML cuando se ejecuta la página.

Con una página HTML estática (*.htm* o *.html* archivo), el servidor cumple un `Web` solicitud leyendo el archivo y enviarlo como-consiste en el explorador. En cambio, cuando un usuario solicita una página Web ASP.NET (*.aspx* archivo), la página se ejecuta como un programa en el servidor Web. Mientras se ejecuta la página, puede realizar cualquier tarea que requiera su sitio Web, incluido calcular valores, leer o escribir la información de la base de datos o una llamada a otros programas. Como resultado, la página genera marcado (por ejemplo, elementos de HTML) dinámicamente y envía este resultado dinámico al explorador.

## <a name="modifying-the-ui"></a>Modificación de la interfaz de usuario

Esta serie de tutoriales que sigan modificando el *Default.aspx* página. Va a modificar la interfaz de usuario que ya se establece mediante la plantilla predeterminada utilizada para crear la aplicación. El tipo de modificaciones que se va a realizar son típicos al crear cualquier aplicación de formularios Web Forms. Para ello, cambia el título, parte del contenido de sustitución y eliminación de contenido predeterminado que no necesite.

1. Abra o cambie a la *Default.aspx* página.
2. Si aparece la página en **diseño** ver, cambiar a **origen** vista.
3. En la parte superior de la página en el `@Page` cambio de la directiva, el `Title` atributo para "Welcome", tal como se muestra resaltado en amarillo a continuación. 

    [!code-aspx[Main](ui_and_navigation/samples/sample1.aspx?highlight=1)]
4. También en el *Default.aspx* página, reemplace todo el contenido predeterminado contenido en el `<asp:Content>` etiquetar para que aparezca el marcado como a continuación. 

    [!code-aspx[Main](ui_and_navigation/samples/sample2.aspx)]
5. Guardar el *Default.aspx* página seleccionando **guardar Default.aspx** desde el **archivo** menú.

   Resultante *Default.aspx* página aparecerá como sigue: 

[!code-aspx[Main](ui_and_navigation/samples/sample3.aspx)]

En el ejemplo, ha establecido la `Title` atributo de la `@Page` directiva. Cuando se muestre el código HTML en un explorador, el código de servidor `<%: Page.Title %>` se resuelve en el contenido incluido en el `Title` atributo.

La página de ejemplo incluye los elementos básicos que constituyen una página Web ASP.NET. La página contiene texto estático que es posible que tenga en una página HTML, junto con los elementos que son específicas de ASP.NET. El contenido incluido en el *Default.aspx* página se integrará con el contenido de la página principal, que se explicará más adelante en este tutorial.

### <a name="page-directive"></a>@Page Directiva

ASP.NET Web Forms normalmente contienen directivas que le permiten especificar información de las propiedades y configuración de página de la página. Las directivas se utilizan por ASP.NET como instrucciones acerca de cómo procesar la página, pero no se representan como parte del marcado que se envía al explorador.

La directiva más frecuente es la `@Page` directiva, que le permite especificar muchas opciones de configuración de la página, incluido lo siguiente:

1. El servidor de lenguaje de programación de código en la página, como C#.
2. Si la página es una página con el código de servidor directamente en la página, que se llama a una página de archivo único, o si es una página con el código en un archivo de clase independiente, que se llama a una página de código subyacente.
3. Si la página tiene una página maestra asociada y, por tanto, debe tratarse como una página de contenido.
4. Depuración y opciones de seguimiento.

Si no incluye un `@Page` la directiva en la página, o si la directiva no incluye una configuración específica, una configuración se heredará la *Web.config* archivo de configuración o desde el *Machine.config* archivo de configuración. El *Machine.config* archivo proporciona una configuración adicional para todas las aplicaciones que se ejecutan en un equipo.

> [!NOTE] 
> 
> El *Machine.config* también proporciona detalles sobre todas las configuraciones posibles.


### <a name="web-server-controls"></a>Controles de servidor Web

En la mayoría de las aplicaciones de formularios Web Forms de ASP.NET, agregará controles que permiten al usuario interactuar con la página, como botones, cuadros de texto, listas y así sucesivamente. Estos controles de servidor Web son similares a los botones HTML y elementos de entrada. Sin embargo, se procesan en el servidor, lo que le permite utilizar código de servidor para establecer sus propiedades. Estos controles también provocan eventos que puede controlar en código del servidor.

Controles de servidor utilizan una sintaxis especial que ASP.NET reconoce cuando se ejecuta la página. El nombre de etiqueta para controles de servidor ASP.NET comienza con un `asp:` prefijo. Esto permite a reconocer y procesar estos controles de servidor ASP.NET. El prefijo podría ser diferente si el control no forma parte de .NET Framework. Además el `asp:` prefijo, controles de servidor ASP.NET también incluyen el `runat="server"` atributo y un `ID` que puede usar para hacer referencia al control de código de servidor.

Cuando se ejecuta la página, ASP.NET identifica los controles de servidor y ejecuta el código que está asociado a esos controles. Muchos controles representan HTML u otro elemento de marcado en la página cuando se muestre en un explorador.

### <a name="server-code"></a>Código de servidor

La mayoría de las aplicaciones de formularios Web Forms ASP.NET incluyen código que se ejecuta en el servidor cuando se procesa la página. Como se mencionó anteriormente, el código de servidor puede utilizarse para llevar a cabo distintas de las cosas, como agregar datos a un control ListView. ASP.NET admite muchos idiomas para ejecutarse en el servidor, incluidos C#, Visual Basic, J# y otros usuarios.

ASP.NET admite dos modelos para escribir el código del servidor para una página Web. En el modelo de único archivo, el código de la página está en un elemento de script que incluye la etiqueta de apertura del `runat="server"` atributo. Como alternativa, puede crear el código de la página en un archivo de clase independiente, que se conoce como el modelo de código subyacente. En este caso, la página de formularios Web Forms ASP.NET generalmente no contiene ningún código de servidor. En su lugar, el `@Page` directiva incluye información que vincula la *.aspx* página con su archivo de código subyacente asociado.

El `CodeBehind` atributos contenidos en el `@Page` directiva especifica el nombre del archivo de clase independiente y el `Inherits` atributo especifica el nombre de la clase dentro del archivo de código subyacente que corresponde a la página.

### <a name="updating-the-master-page"></a>Actualizar la página maestra

En los formularios Web Forms de ASP.NET, las páginas maestras permiten crear un diseño coherente para las páginas en la aplicación. Una sola página maestro define la apariencia y el comportamiento estándar que desea para todas las páginas (o un grupo de páginas) de la aplicación. A continuación, puede crear páginas de contenido individuales que contienen el contenido que desea mostrar, como se explicó anteriormente. Cuando los usuarios solicitan las páginas de contenido, ASP.NET las combina con la página maestra para producir el resultado que combine el diseño de la página maestra con el contenido de la página de contenido.

El nuevo sitio necesita un logotipo único deben mostrarse en cada página. Para agregar este logotipo, puede modificar el código HTML en la página maestra.

1. En **el Explorador de soluciones**, busque y abra el **Site.Master** página.
2. Si la página está en **diseño** ver, cambiar a **origen** vista.
3. Actualice la página maestra por **modificar o agregar** el marcado resaltado en amarillo: 

    [!code-aspx[Main](ui_and_navigation/samples/sample4.aspx?highlight=9,49,76-81,87)]

Este código HTML mostrará la imagen llamada *archivo logo.jpg* desde el *imágenes* carpeta de la aplicación Web, que se agregará más adelante. Cuando se muestra una página que usa la página maestra en un explorador, se mostrará el logotipo. Si un usuario hace clic en el logotipo, el usuario se desplazará hasta el *Default.aspx* página. La etiqueta delimitadora HTML `<a>` ajusta el control de servidor de la imagen y permita que la imagen que se incluya como parte del vínculo. El `href` para la etiqueta delimitadora especifica la raíz del atributo "`~/`" del sitio Web como la ubicación del vínculo. De forma predeterminada, el *Default.aspx* página se muestra cuando el usuario navega a la raíz del sitio Web. El **imagen** `<asp:Image>` control de servidor incluye las propiedades de la suma, como `BorderStyle`, que se representan como HTML cuando se muestre en un explorador.

### <a name="master-pages"></a>Páginas maestras

Una página maestra es un archivo ASP.NET con la extensión .master (por ejemplo, *Site.Master*) con un diseño predefinido que puede incluir texto estático, los elementos HTML y controles de servidor. La página maestra se identifica mediante un especial `@Master` directiva que reemplaza el `@Page` directiva que se usa para normal *.aspx* páginas.

Además el `@Master` la directiva, la página principal también contiene todos los elementos HTML de nivel superior para una página, como `html`, `head`, y `form`. Por ejemplo, en la página maestra que agregó anteriormente, use un elemento HTML `table` para el diseño, un `img` (elemento) para el logotipo de la compañía, texto estático y controles de servidor para controlar la pertenencia comunes para su sitio. Puede usar cualquier HTML y ASP.NET como parte de la página maestra.

Además de texto estático y controles que va a aparecer en todas las páginas, la página maestra también incluye uno o varios **ContentPlaceHolder** controles. Estos controles de marcador de posición definen las regiones donde se mostrará el contenido reemplazable. A su vez, el contenido reemplazable se define en las páginas de contenido, como *Default.aspx*, usando la **contenido** control de servidor.

#### <a name="adding-image-files"></a>Agregar archivos de imagen

La imagen de logotipo que se hace referencia anteriormente, junto con todas las imágenes de producto, debe agregarse a la aplicación Web para que se aprecia cuando el proyecto se muestra en un explorador.

#### <a name="download-from-msdn-samples-site"></a>Descargar desde el sitio de ejemplos de MSDN:

[Introducción a ASP.NET 4.5 Web Forms y Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)

La descarga incluye los recursos en el *WingtipToys-activos* carpeta que se usan para crear la aplicación de ejemplo.

1. Si aún no lo ha hecho, descargue los archivos de ejemplo comprimidos mediante el vínculo anterior desde el sitio de ejemplos de MSDN.
2. Una vez descargado, abra el archivo .zip y copie el contenido en una carpeta local en el equipo.
3. Busque y abra el *WingtipToys-activos* carpeta.
4. Arrastrando y colocando, copie el *catálogo* carpeta desde la carpeta local a la raíz del proyecto de aplicación Web en el **el Explorador de soluciones** de Visual Studio. 

    ![Interfaz de usuario y navegación - copiar archivos](ui_and_navigation/_static/image1.png)
5. A continuación, cree una carpeta nueva denominada *imágenes* haciendo clic con el **WingtipToys** proyecto **el Explorador de soluciones** y seleccionando **agregar**  - &gt; **Nueva carpeta**.
6. Copia el *archivo logo.jpg* de archivos desde el *WingtipToys-activos* carpeta en **Explorador de archivos** a la *imágenes* carpeta de la aplicación Web Project en **el Explorador de soluciones** de Visual Studio.
7. Haga clic en el **mostrar todos los archivos** opción en la parte superior de **el Explorador de soluciones** para actualizar la lista de archivos si no ve los nuevos archivos.  
  
    **El Explorador de soluciones** ahora muestra los archivos de proyecto actualizado. 

    ![Interfaz de usuario y navegación - Explorador de soluciones](ui_and_navigation/_static/image2.png)

### <a name="adding-pages"></a>Adición de páginas

Antes de Agregar navegación a la aplicación Web, primero agregará dos páginas nuevas que podrá navegar a. Más adelante en esta serie de tutoriales, mostrará productos y sus detalles en estas páginas nuevas.

1. En **el Explorador de soluciones**, haga clic en **WingtipToys**, haga clic en **agregar**y, a continuación, haga clic en **nuevo elemento**.   
 Se abrirá el cuadro de diálogo **Agregar nuevo elemento**.
2. Seleccione el **Visual C#**  - &gt; **Web** grupo de plantillas de la izquierda. A continuación, seleccione **formulario Web Forms con página maestra** desde la parte central de lista y asígnele el nombre *ProductList.aspx*. 

    ![Interfaz de usuario y navegación - Agregar nuevo elemento de cuadro de diálogo](ui_and_navigation/_static/image3.png)
3. Seleccione **Site.Master** para asociar la página maestra recién creado *.aspx* página. 

    ![Interfaz de usuario y navegación - Seleccionar página maestra](ui_and_navigation/_static/image4.png)
4. Agregar una página adicional denominada *ProductDetails.aspx* siguiendo estos mismos pasos.

### <a name="updating-bootstrap"></a>Actualización de Bootstrap

Usan las plantillas de proyecto de Visual Studio 2013 [Bootstrap](http://getbootstrap.com/), un marco de trabajo de diseño y creación de temas creado por Twitter. Bootstrap usa CSS3 para proporcionar un diseño dinámico, lo que significa que los diseños pueden adaptarse dinámicamente a los tamaños de ventana de explorador diferente. También puede usar características de creación de temas de Bootstrap para llevar a cabo fácilmente un cambio en apariencia y comportamiento de la aplicación. De forma predeterminada, la plantilla de aplicación Web ASP.NET en Visual Studio 2013 incluye Bootstrap como un paquete de NuGet.

En este tutorial, cambiará la apariencia y comportamiento de la aplicación Wingtip Toys reemplazando los archivos CSS de Bootstrap.

1. En **el Explorador de soluciones**, abra el *contenido* carpeta.
2. Haga clic en el *bootstrap.css* de archivo y cambie su nombre a *bootstrap original.css*.
3. Cambiar el nombre de la *bootstrap.min.css* a *bootstrap original.min.css*.
4. En **el Explorador de soluciones**, haga clic en el *contenido* carpeta y seleccione **Abrir carpeta en el Explorador de archivos**.  
   Se mostrará el Explorador de archivos. Guardará descargado archivos CSS bootstrap para esta ubicación.
5. En el explorador, vaya a [ https://bootswatch.com/3/ ](https://bootswatch.com/3/).
6. Desplácese a la ventana del explorador hasta que vea el tema Cerulean. 

    ![Interfaz de usuario y navegación - tema Cerulean](ui_and_navigation/_static/image5.png)
7. Descargar tanto el *bootstrap.css* archivo y la *bootstrap.min.css* del archivo a la *contenido* carpeta. Utilice la ruta de acceso a la carpeta de contenido que se muestra en el **Explorador de archivos** ventana que abrió anteriormente.
8. En **Visual Studio** en la parte superior de **el Explorador de soluciones**, seleccione el **mostrar todos los archivos** opción para mostrar los nuevos archivos en la carpeta de contenido. 

    ![Interfaz de usuario y navegación - Explorador de soluciones](ui_and_navigation/_static/image6.png)

   Verá los dos nuevos archivos CSS en el **contenido** carpeta, pero tenga en cuenta que el icono junto a cada nombre de archivo está atenuado. Esto significa que el archivo no se ha agregado al proyecto.
9. Haga clic en el *bootstrap.css* y *bootstrap.min.css* archivos y seleccione **incluir en el proyecto**.   
   Al ejecutar la aplicación Wingtip Toys más adelante en este tutorial, se mostrará la nueva interfaz de usuario.

> [!NOTE] 
> 
> La plantilla de aplicación Web de ASP.NET utiliza el *Bundle.config* archivo en la raíz del proyecto para almacenar la ruta de acceso de los archivos CSS de Bootstrap.


### <a name="modifying-the-default-navigation"></a>Modificar el panel de navegación predeterminado

El panel de navegación predeterminada para todas las páginas en la aplicación se puede modificar cambiando el elemento de lista sin ordenar de navegación que se encuentra en la *Site.Master* página.

1. En **el Explorador de soluciones**, busque y abra el *Site.Master* página.
2. Agregar el vínculo de navegación adicionales que se resalta en amarillo en la lista sin ordenar que se muestra a continuación:   

    [!code-html[Main](ui_and_navigation/samples/sample5.html?highlight=5)]

Como puede ver en el código HTML anterior, modificó cada elemento de línea `<li>` que contiene una etiqueta delimitadora `<a>` con un vínculo `href` atributo. Cada `href` apunta a una página en la aplicación Web. En el explorador, cuando un usuario hace clic en uno de estos vínculos (como **productos**), se le remitirá a la página de contenido en el `href` (como **ProductList.aspx**). Se ejecutará la aplicación al final de este tutorial.

> [!NOTE] 
> 
> La tilde (`~`) carácter se utiliza para especificar que el `href` ruta comienza en la raíz del proyecto.


### <a name="adding-a-data-control-to-display-navigation-data"></a>Agregar un Control de datos para mostrar los datos de exploración

A continuación, agregará un control para mostrar todas las categorías de la base de datos. Cada categoría actuará como un vínculo a la *ProductList.aspx* página. Cuando un usuario hace clic en un vínculo de categoría en el explorador, que se vaya a la página de productos y ver solo los productos asociados con la categoría seleccionada.

Deberá usar un **ListView** control para mostrar todas las categorías contenidas en la base de datos. Para agregar un **ListView** control a la página maestra:

1. En el *Site.Master* página, agregue las siguientes resaltadas `<div>` elemento **después** el `<div>` elemento que contiene el `id="TitleContent"` que agregó anteriormente:  

    [!code-aspx[Main](ui_and_navigation/samples/sample6.aspx?highlight=7-21)]

Este código mostrará todas las categorías de la base de datos. El **ListView** control muestra cada nombre de categoría como texto del vínculo e incluye un vínculo a la *ProductList.aspx* página con un valor de cadena de consulta que contenga el `ID` de la categoría. Estableciendo el `ItemType` propiedad en el **ListView** controlar, la expresión de enlace de datos `Item` está disponible en el `ItemTemplate` nodo y el control pasa a ser fuertemente tipados. Puede seleccionar los detalles de la `Item` objeto con IntelliSense, como especificar el `CategoryName`. Este código se encuentra dentro del contenedor `<%#: %>` que marca una expresión de enlace de datos. Agregando el (:) al final de la `<%#` prefijo, el resultado de la expresión de enlace de datos está codificada en HTML. Cuando el resultado está codificada en HTML, la aplicación está mejor protegida frente a sitios por inyección de código (XSS) y ataques de inyección de código HTML de script.

> [!NOTE] 
> 
> **Sugerencia**
> 
> Cuando se agrega código escribiendo durante el desarrollo, puede estar seguro de que un miembro válido de un objeto se encuentra como fuertemente tipada controles de datos muestran a los miembros disponibles en función de IntelliSense. IntelliSense ofrece opciones de código adecuadas al contexto a medida que escribe código, como propiedades, métodos y objetos.


En el paso siguiente, implementará la `GetCategories` método para recuperar los datos.

### <a name="linking-the-data-control-to-the-database"></a>Vincular el Control de datos a la base de datos

Antes de poder mostrar datos en el control de datos, debe vincular el control de datos a la base de datos. Para crear el vínculo, puede modificar el código subyacente de la *Site.Master.cs* archivo.

1. En **el Explorador de soluciones**, haga clic en el *Site.Master* página y, a continuación, haga clic en **ver código**. El *Site.Master.cs* archivo se abre en el editor.
2. Cerca del principio de la *Site.Master.cs* , agregue dos espacios de nombres adicionales para que todos los espacios de nombres incluye aparecerán como sigue:  

    [!code-csharp[Main](ui_and_navigation/samples/sample7.cs?highlight=8-9)]
3. Agregue el resaltado `GetCategories` método después de la `Page_Load` controlador de eventos como sigue:  

    [!code-csharp[Main](ui_and_navigation/samples/sample8.cs?highlight=6-11)]

El código anterior se ejecuta cuando se cargue cualquier página que usa la página maestra en el explorador. El `ListView` control (con nombre "categoryList") que ha agregado anteriormente en este tutorial usa el enlace de modelos para seleccionar los datos. En el marcado de la `ListView` control se configura el control `SelectMethod` propiedad a la `GetCategories` método, mostrado anteriormente. El `ListView` las llamadas de control del `GetCategories` ciclo de método en el momento adecuado en la vida de la página y se enlaza automáticamente los datos devueltos. Aprenderá más acerca de enlace de datos en el siguiente tutorial.

### <a name="running-the-application-and-creating-the-database"></a>Ejecutar la aplicación y crear la base de datos

Anteriormente en esta serie de tutoriales se crea una clase de inicializador (denominada "ProductDatabaseInitializer") y se especifica esta clase en el *global.asax.cs* archivo. Entity Framework generará la base de datos cuando se ejecuta la primera vez que la aplicación porque el `Application_Start` método contenido en el *global.asax.cs* archivo llamará a la clase de inicializador. La clase de inicializador usará las clases del modelo (`Category` y `Product`) que agregó anteriormente en esta serie de tutoriales para crear la base de datos.

1. En **el Explorador de soluciones**, haga clic en el *Default.aspx* página y seleccione **establecer como página principal**.
2. En Visual Studio, presione **F5**.   
 Se tardará un poco de tiempo para configurar todo durante esta primera ejecución.   
    ![Interfaz de usuario y navegación - explorador Windows](ui_and_navigation/_static/image7.png)  
 Al ejecutar la aplicación, la aplicación se compilará y la base de datos denominada *wingtiptoys.mdf* se creará en el *aplicación\_datos* carpeta. En el explorador, verá un menú de navegación de la categoría. Este menú se generó al recuperar las categorías de la base de datos. En el siguiente tutorial, implementará el panel de navegación.
3. Cierre el explorador para detener la aplicación en ejecución.

### <a name="reviewing-the-database"></a>Revisión de la base de datos

Abra el *Web.config* de archivo y examine la sección de la cadena de conexión. Puede ver que el `AttachDbFilename` valor en la cadena de conexión apunta a la `DataDirectory` para el proyecto de aplicación Web. El valor `|DataDirectory|` es un valor reservado que representa el *aplicación\_datos* carpeta del proyecto. Esta carpeta es donde se encuentra la base de datos que se creó a partir de las clases de entidad.

[!code-xml[Main](ui_and_navigation/samples/sample9.xml)]

> [!NOTE] 
> 
> Si el *aplicación\_datos* carpeta no está visible o si la carpeta está vacía, seleccione el **actualizar** icono y, a continuación, el **mostrar todos los archivos** situado en la parte superior de la **El Explorador de soluciones** ventana. Expandir el ancho de la **el Explorador de soluciones** windows es posible que deba mostrar todos los iconos disponibles.


Ahora puede inspeccionar los datos contenidos en el *wingtiptoys.mdf* archivo de base de datos mediante el uso de la **Explorador de servidores** ventana.

1. Expanda el *aplicación\_datos* carpeta. Si el *aplicación\_datos* carpeta no está visible, consulte la nota anterior.
2. Si el *wingtiptoys.mdf* el archivo de base de datos no está visible, seleccione el **actualizar** icono y, a continuación, el **mostrar todos los archivos** situado en la parte superior de la **el Explorador de soluciones**  ventana.
3. Haga clic en el *wingtiptoys.mdf* el archivo de base de datos y seleccione **abierto**.  
    **Explorador de servidores** se muestra. 

    ![Interfaz de usuario y navegación - Explorador de servidores](ui_and_navigation/_static/image8.png)
4. Expanda el *tablas* carpeta.
5. Haga clic en el **productos**de tabla y seleccione **mostrar datos de tabla**.  
 El **productos** se muestra la tabla. 

    ![Interfaz de usuario y navegación - tabla de productos](ui_and_navigation/_static/image9.png)
6. Esta vista le permite ver y modificar los datos en el **productos** tabla manualmente.
7. Cerrar la **productos** ventana de la tabla.
8. En el **Explorador de servidores**, haga clic en el **productos** nuevo de la tabla y seleccione **Abrir definición de tabla**.  
 Los datos de diseño para el **productos** se muestra la tabla. 

    ![Interfaz de usuario y navegación - diseño de productos](ui_and_navigation/_static/image10.png)
9. En el **T-SQL** pestaña verá la instrucción DDL de SQL que se usó para crear la tabla. También puede usar la interfaz de usuario en el **diseño** ficha para modificar el esquema.
10. En el **Explorador de servidores**, haga clic en **WingtipToys** de base de datos y seleccione **cerrar conexión**.   
 Separando la base de datos desde Visual Studio, el esquema de base de datos podrá modificarse más adelante en esta serie de tutoriales.
11. Vuelva a **el Explorador de soluciones**seleccionando el **el Explorador de soluciones** en la parte inferior de la **Explorador de servidores** ventana.

## <a name="summary"></a>Resumen

En este tutorial de la serie se han agregado alguna interfaz de usuario básica, gráficos, páginas y navegación. Además, se ejecutó la aplicación Web, que crea la base de datos de las clases de datos que agregó en el tutorial anterior. También ve el contenido de la *productos* tabla de la base de datos mediante la visualización de la base de datos directamente. En el siguiente tutorial, mostrará los elementos de datos y los detalles de la base de datos.

## <a name="additional-resources"></a>Recursos adicionales

[Introducción a la programación de páginas Web de ASP.NET](https://msdn.microsoft.com/library/ms178125.aspx)   
[Introducción a los controles de servidor Web de ASP.NET](https://msdn.microsoft.com/library/zsyt68f1.aspx)   
[Tutorial CSS](http://www.w3schools.com/css/default.asp)

> [!div class="step-by-step"]
> [Anterior](create_the_data_access_layer.md)
> [Siguiente](display_data_items_and_details.md)
