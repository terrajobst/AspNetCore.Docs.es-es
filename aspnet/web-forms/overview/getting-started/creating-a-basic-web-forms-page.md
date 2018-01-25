---
uid: web-forms/overview/getting-started/creating-a-basic-web-forms-page
title: "Crear un ASP.NET básica 4.5 Web Forms página en Visual Studio 2013 | Documentos de Microsoft"
author: Erikre
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/03/2014
ms.topic: article
ms.assetid: a2f1c635-0817-4a9a-8c13-d5b5d29727c0
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/creating-a-basic-web-forms-page
msc.type: authoredcontent
ms.openlocfilehash: 6b699cc939292b7ab0167dba7cfa6a00b681ef3a
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="creating-a-basic-aspnet-45-web-forms-page-in-visual-studio-2013"></a>Crear un ASP.NET básica 4.5 Web Forms página en Visual Studio 2013
====================
Por [Erik Reitan](https://github.com/Erikre)

Este tutorial proporciona una introducción al entorno de desarrollo Web en [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) y en [Microsoft Visual Studio Express 2013 para Web](https://www.microsoft.com/visualstudio/11/downloads#express-web). En este tutorial le guía por la creación de una página de formularios Web Forms ASP.NET simple y muestra las técnicas básicas de creación de una nueva página, agregar controles y escribir código.

Las tareas ilustradas en este tutorial incluyen:

- Crear un proyecto de aplicación de formularios Web Forms de sistema de archivos.
- Familiarizarse con Visual Studio.
- Crear una página ASP.NET.
- Agregar controles.
- Agregar controladores de eventos.
- Ejecutar y probar una página desde Visual Studio.

## <a name="prerequisites"></a>Requisitos previos


Para poder completar este tutorial, necesitará:

- [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) o [Microsoft Visual Studio Express 2013 para Web](https://www.microsoft.com/visualstudio/11/downloads#express-web). .NET Framework se instala automáticamente. 

    > [!NOTE] 
    > 
    > Microsoft Visual Studio 2013 y Microsoft Visual Studio Express 2013 para Web a menudo se conocerán como Visual Studio a lo largo de esta serie de tutoriales.  
    >   
    > Si se utiliza Visual Studio, en este tutorial se da por supuesto que ha seleccionado la **desarrollo Web** colección de valores de la primera vez que inicia Visual Studio. Para obtener más información, consulte [Cómo: seleccionar configuración de entorno de desarrollo Web](https://msdn.microsoft.com/library/ff521558.aspx).


## <a name="creating-a-web-application-project-and-a-page"></a>Crear un proyecto de aplicación Web y una página

<a id="sectionToggle0"></a>

En esta parte del tutorial, creará un proyecto de aplicación Web y agregue una nueva página a él. También agregará texto HTML y ejecutará la página en el explorador.

### <a name="to-create-a-web-application-project"></a>Para crear un proyecto de aplicación Web

1. Abra Microsoft Visual Studio.
2. En el menú **Archivo**, seleccione **Nuevo proyecto**.  
    ![Menú archivo](creating-a-basic-web-forms-page/_static/image1.png)

    Aparecerá el cuadro de diálogo **Nuevo proyecto** .
3. Seleccione el **plantillas**  - &gt; **Visual C#**  - &gt; **Web** grupo de plantillas de la izquierda.
4. Elija la **aplicación Web ASP.NET** plantilla en la columna central.
5. Denomine el proyecto ***BasicWebApp*** y haga clic en el **Aceptar** botón.   
![Cuadro de diálogo nuevo proyecto](creating-a-basic-web-forms-page/_static/image2.png)
6. A continuación, seleccione la **formularios Web Forms** plantilla y haga clic en el **Aceptar** botón para crear el proyecto.  
![Cuadro de diálogo nuevo proyecto de ASP.NET](creating-a-basic-web-forms-page/_static/image3.png)  

    Visual Studio crea un nuevo proyecto que incluye funcionalidad creada previamente de basado en la plantilla de formularios Web Forms. No solo ofrece a un *Home.aspx* página, un *About.aspx* página, un *Contact.aspx* página, pero también incluye la funcionalidad de pertenencia que registra los usuarios y guarda sus credenciales para que puede iniciar sesión en el sitio Web. Cuando se crea una nueva página, de forma predeterminada Visual Studio muestra la página en **origen** vista, donde puede ver los elementos de la página HTML. La siguiente ilustración muestra lo que se ve en **origen** ver si ha creado una nueva página Web denominada *BasicWebApp.aspx*.  
    ![Vista Código fuente](creating-a-basic-web-forms-page/_static/image4.png)


### <a name="a-tour-of-the-visual-studio-web-development-environment"></a>Un paseo por el entorno de desarrollo Web de Visual Studio


Antes de continuar modificando la página, es útil para familiarizarse con el entorno de desarrollo de Visual Studio. En la siguiente ilustración muestra las ventanas y herramientas que están disponibles en Visual Studio y Visual Studio Express para Web.

> [!NOTE] 
> 
> Este diagrama muestra las ventanas predeterminadas y las ubicaciones de las ventanas. El **vista** menú permite mostrar ventanas adicionales así como reorganizar y cambiar el tamaño de windows para adaptarlo a sus preferencias. Si ya se realizaron cambios en la organización de las ventanas, lo que se ve no coincidirá con la ilustración.


 El entorno de Visual Studio

![Entorno de Visual Studio](creating-a-basic-web-forms-page/_static/image5.png)

### <a name="familiarize-yourself-with-the-web-designer"></a>Familiarizarse con el Diseñador de Web

Examine la ilustración anterior y coinciden con el texto a la lista siguiente, que describe el normalmente usa ventanas y herramientas. (No todas las ventanas y herramientas que se muestran aquí, solo los marcados en la ilustración anterior.)

- Barras de herramientas. Proporcionar comandos para dar formato a texto, búsqueda de texto y así sucesivamente. Algunas barras de herramientas solo están disponibles cuando se trabaja en **diseño** vista.
- **El Explorador de soluciones** ventana. Muestra los archivos y carpetas en la aplicación Web.
- Ventana de documento. Muestra los documentos que se está trabajando en ventanas con fichas. Puede cambiar entre los documentos, haga clic en las pestañas.
- **Propiedades** ventana. Permite cambiar la configuración de la página, los elementos HTML, controles y otros objetos.
- Examine las fichas. Presentan vistas diferentes del mismo documento. **Diseño** vista es una superficie de edición prácticamente WYSIWYG. **Origen** vista es el editor HTML para la página. **División** vista muestra los la **diseño** vista y la **origen** vista para el documento. Trabajará con la **diseño** y **origen** vistas más adelante en este tutorial. Si prefiere abrir páginas Web en **diseño** ver, en la **herramientas** menú, haga clic en **opciones**, seleccione la **Diseñador HTML** nodo y cambie el **Iniciar páginas en** opción.
- **ToolBox**. Proporciona controles y los elementos HTML que pueden arrastrar a la página. **Cuadro de herramientas** elementos se agrupan por función común.
- S **erver explorador**. Muestra las conexiones de base de datos. Si el Explorador de servidores no está visible, en el menú Ver, haga clic en Explorador de servidores.


### <a name="creating-a-new-aspnet-web-forms-page"></a>Crear una nueva página de formularios Web de ASP.NET


Cuando se crea una nueva aplicación de formularios Web Forms con el **aplicación Web ASP.NET** plantilla de proyecto, Visual Studio agrega una página ASP.NET (página de formularios Web Forms) denominada *Default.aspx*, así como otros archivos y las carpetas. Puede usar el *Default.aspx* página como página principal de la aplicación Web. Sin embargo, en este tutorial, creará y trabajar con una nueva página.

### <a name="to-add-a-page-to-the-web-application"></a>Para agregar una página a la aplicación Web


1. Cerrar la *Default.aspx* página. Para ello, haga clic en la ficha que muestra el nombre de archivo y, a continuación, haga clic en la opción Cerrar.
2. En **el Explorador de soluciones**, haga clic en el nombre de aplicación Web (en este tutorial es el nombre de la aplicación **BasicWebSite**) y, a continuación, haga clic en **agregar**  - &gt; **Nuevo elemento**.   
Se abrirá el cuadro de diálogo **Agregar nuevo elemento**.
3. Seleccione el **Visual C#**  - &gt; **Web** grupo de plantillas de la izquierda. A continuación, seleccione **formulario Web** desde la mitad de lista y asígnele el nombre *FirstWebPage.aspx*.   
    ![Agregar cuadro de diálogo nuevo elemento](creating-a-basic-web-forms-page/_static/image6.png)
4. Haga clic en **agregar** para agregar la página web a su proyecto.  
Visual Studio crea la nueva página y lo abre.


### <a name="adding-html-to-the-page"></a>Agregar HTML a la página


En esta parte del tutorial, agregará texto estático a la página.

### <a name="to-add-text-to-the-page"></a>Para agregar texto a la página


1. En la parte inferior de la ventana de documento, haga clic en el **diseño** ficha para cambiar a **diseño** vista.

    La vista Diseño muestra la página actual en un modo WYSIWYG. En este momento, no hay texto ni controles en la página, por lo que la página está en blanco excepto una línea discontinua que forma un rectángulo. Este rectángulo representa un **div** elemento de la página.
2. Haga clic dentro del rectángulo que se indica mediante una línea discontinua.
3. Tipo de **Bienvenido a Visual Web Developer** y presione **ENTRAR** dos veces.

    La ilustración siguiente muestra el texto que escribió en **diseño** vista.

    ![Texto de bienvenida en la vista Diseño](creating-a-basic-web-forms-page/_static/image7.png "texto de bienvenida en la vista Diseño")
4. Cambie a **origen** vista.

    Puede ver el código HTML en **origen** vista que ha creado al escribir en **diseño** vista.  
    ![Página Web con texto estático](creating-a-basic-web-forms-page/_static/image8.png)


### <a name="running-the-page"></a>Ejecución de la página


Antes de continuar agregando controles a la página, también puede ejecutarla.

### <a name="to-run-the-page"></a>Para ejecutar la página


1. En **el Explorador de soluciones**, haga clic en *FirstWebPage.aspx* y seleccione **establecer como página de inicio**.
2. Presione **CTRL+F5** para ejecutar la página.

    La página se muestra en el explorador. Aunque la página creada tiene una extensión de nombre de archivo de *.aspx*, actualmente se ejecuta como cualquier página HTML.

    Para mostrar una página en el explorador puede hacer clic en la página de **el Explorador de soluciones** y seleccione **ver en el explorador**.
3. Cierre el explorador para detener la aplicación Web.


## <a name="adding-and-programming-controls"></a>Agregar y programación de controles


<a id="sectionToggle1"></a>

Ahora agregará controles de servidor a la página. Controles de servidor, como botones, etiquetas, cuadros de texto y otros controles familiares, proporcionan capacidades de procesamiento de formularios típicas para las páginas de formularios Web Forms. Sin embargo, puede programar los controles con código que se ejecuta en el servidor, en lugar de en el cliente.

Agregará un [botón](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) (control), un [cuadro de texto](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) (control) y un [etiqueta](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) el control a la página y escribir código para controlar la [haga clic en](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) eventos para el [botón](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) control.

### <a name="to-add-controls-to-the-page"></a>Para agregar controles a la página


1. Haga clic en el **diseño** ficha para cambiar a **diseño** vista.
2. Coloque el punto de inserción al final de la **Bienvenido a Visual Web Developer** texto y presione **ENTRAR** cinco o más veces para hacer algo de espacio en el **div** cuadro del elemento.
3. En el **cuadro de herramientas**, expanda la **estándar** grupo si aún no está expandido.  
Tenga en cuenta que puede que necesite expandir la **cuadro de herramientas** ventana de la izquierda para verlo.
4. Arrastre un [cuadro de texto](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) controlar hasta la página y colóquela en el medio de la **div** seudoelemento que tenga **Bienvenido a Visual Web Developer** en la primera línea.
5. Arrastre un [botón](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) controlar hasta la página y colóquelo a la derecha de la [TextBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) control.
6. Arrastre un [etiqueta](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) controlar hasta la página y colóquelo en una línea independiente a continuación el [botón](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) control.
7. Coloque el punto de inserción anterior la [cuadro de texto](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) de control y, a continuación, escriba **escriba su nombre:** .

    Este texto HTML estático es el título de la [TextBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) control. Puede mezclar HTML estático y controles de servidor en la misma página. En la siguiente ilustración muestra cómo aparecen los tres controles en **diseño** vista.

    ![Tres controles en la vista Diseño](creating-a-basic-web-forms-page/_static/image9.png "tres controles en la vista Diseño")


### <a name="setting-control-properties"></a>Establecer las propiedades de Control


Visual Studio ofrece varias maneras de establecer las propiedades de controles en la página. En esta parte del tutorial, establecerá propiedades en ambos **diseño** vista y **origen** vista.

### <a name="to-set-control-properties"></a>Para establecer las propiedades de control


1. En primer lugar, mostrar la **propiedades** windows mediante la selección de la **vista** menú -&gt; **otras ventanas**  - &gt; **Ventana propiedades**. Como alternativa, puede seleccionar **F4** para mostrar la **propiedades** ventana.
2. Seleccione el [botón](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) (control) y, a continuación, en la **propiedades** ventana, establezca el valor de **texto** a **nombre para mostrar**. El texto especificado aparece en el botón en el diseñador, como se muestra en la siguiente ilustración.

    ![Establecer texto del botón](creating-a-basic-web-forms-page/_static/image10.png "texto del botón establecer")
3. Cambie a **origen** vista.

    **Origen** vista muestra el código HTML de la página, incluidos los elementos creados por Visual Studio para los controles de servidor. Los controles se declaran mediante la sintaxis HTML, salvo que las etiquetas utilizan el prefijo **asp:** e incluyen el atributo **runat =&quot;server&quot;**.

    Propiedades del control se declaran como atributos. Por ejemplo, al establecer el [texto](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.text.aspx) propiedad para la [botón](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) de control, en el paso 1, estaba estableciendo realmente el **texto** atributo en el marcado del control.

    > [!NOTE] 
    > 
    > Todos los controles que están dentro de un **formulario** elemento, que también tiene el atributo **runat =&quot;server&quot;**. El **runat =&quot;server&quot;**  atributo y el **asp:** prefijo control etiquetas marcan los controles para que ASP.NET los procese en el servidor cuando se ejecuta la página. El código fuera de  **&lt;formulario runat =&quot;server&quot; &gt;**  y  **&lt;script runat =&quot;server&quot; &gt;**  elementos se envía sin cambios en el explorador, que es el motivo por el código ASP.NET debe estar dentro de un elemento cuya etiqueta de apertura contiene la **runat =&quot;server&quot;**  atributo.
4. A continuación, agregará una propiedad adicional para la [etiqueta](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) control. Coloque el punto de inserción directamente después de **asp: Label** en el  **&lt;asp: Label&gt;**  etiqueta y, a continuación, presione **barra espaciadora**.

    Aparece una lista desplegable que muestra la lista de propiedades disponibles que se pueden definir para un [etiqueta](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) control. Esta característica, denomina **IntelliSense**, le ayuda a en **origen** vista con la sintaxis de controles de servidor, los elementos HTML y otros elementos de la página. La siguiente ilustración muestra la **IntelliSense** la lista desplegable para la [etiqueta](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) control.

    ![Atributos de IntelliSense](creating-a-basic-web-forms-page/_static/image11.png "atributos de IntelliSense")
5. Seleccione **ForeColor** y, a continuación, escriba un signo igual.

    IntelliSense muestra una lista de colores.

    > [!NOTE] 
    > 
    > Puede mostrar un **IntelliSense** lista de desplegable en cualquier momento presionando **CTRL+J** al ver el código.
6. Seleccione un color para el  **[etiqueta](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx)**  texto del control. Asegúrese de que selecciona un color que es suficientemente oscuro para leer el texto sobre un fondo blanco.

    El **ForeColor** atributo se completa con el color que ha seleccionado, incluidas las comillas de cierre.


### <a name="programming-the-button-control"></a>El Control de botón de programación


En este tutorial, escribirá código que lee el nombre que el usuario escribe en el cuadro de texto y, a continuación, muestra el nombre de la [etiqueta](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) control.

### <a name="add-a-default-button-event-handler"></a>Agregar un controlador de eventos de botón predeterminado


1. Cambie a **diseño** vista.
2. Haga doble clic en el [botón](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) control.

    De forma predeterminada, Visual Studio, se activa en un archivo de código subyacente y crea un controlador de eventos de esqueleto para el [botón](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) evento predeterminado del control, el [haga clic en](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) eventos. El archivo de código subyacente separa el código de interfaz de usuario (por ejemplo, HTML) desde el código de servidor (por ejemplo, C#).   
El cursor se coloca a agregar código para este controlador de eventos.

    > [!NOTE] 
    > 
    > Haga doble clic en un control en **diseño** vista es simplemente una de varias maneras de crear controladores de eventos.
3. Dentro de la **Button1\_haga clic en** controlador de eventos, escriba **Label1** seguido por un punto (**.**).

    Cuando se escribe un punto después la **identificador** de la etiqueta (**Label1**), Visual Studio muestra una lista de miembros disponibles para la [etiqueta](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) controlar, tal y como se muestra en el siguiente ilustración. Un miembro normalmente una propiedad, método o evento.

    ![IntelliSense en la vista código](creating-a-basic-web-forms-page/_static/image12.png "IntelliSense en la vista de código")
4. Finalizar la **haga clic en** controlador de eventos para el botón de forma que indique como se muestra en el ejemplo de código siguiente.

    [!code-csharp[Main](creating-a-basic-web-forms-page/samples/sample1.cs?highlight=3)]

    [!code-vb[Main](creating-a-basic-web-forms-page/samples/sample2.vb?highlight=2)]
5. Volver a ver el **origen** vista del código de marcado HTML haciendo clic en *FirstWebPage.aspx* en el **el Explorador de soluciones** y seleccionando **vista Marcado**.
6. Desplácese hasta la  **&lt;asp: botón&gt;**  elemento. Tenga en cuenta que la  **&lt;asp: botón&gt;**  elemento ahora tiene el atributo **onclick =&quot;Button1\_haga clic en&quot;**.

    Este atributo enlaza el botón [haga clic en](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) evento al método de controlador se codificó en el paso anterior.

    Métodos de controlador de eventos pueden tener cualquier nombre; el nombre que aparece es el nombre predeterminado creado por Visual Studio. Lo importante es que el nombre se usa para la **OnClick** atributo en el código HTML debe coincidir con el nombre de un método definido en el código subyacente.


### <a name="running-the-page"></a>Ejecución de la página


Ahora puede probar los controles de servidor en la página.

### <a name="to-run-the-page"></a>Para ejecutar la página


1. Presione **CTRL+F5** para ejecutar la página en el explorador. Si se produce un error, volver a comprobar los pasos anteriores.
2. Escriba un nombre en el cuadro de texto y haga clic en el **nombre para mostrar** botón.

    El nombre especificado se muestra en el [etiqueta](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) control. Tenga en cuenta que, al hacer clic en el botón, la página se envía al servidor Web. ASP.NET, a continuación, vuelve a crear la página, se ejecuta el código (en este caso, el [botón](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) del control [haga clic en](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) ejecuciones de controlador de eventos) y, a continuación, envía la nueva página en el explorador. Si observa la barra de estado en el explorador, puede ver que la página realiza un de ida y vuelta al servidor Web cada vez que haga clic en el botón.
3. En el explorador, ver el código fuente de la página que se está ejecutando, haga clic en la página y seleccionar **ver código fuente**.

    En el código fuente de página, verá HTML sin ningún código de servidor. En concreto, no ve el  **&lt;asp:&gt;**  elementos que estaba trabajando con en **origen** vista. Cuando se ejecuta la página, ASP.NET procesa los controles de servidor y representa los elementos HTML a la página que realizan las funciones que representan el control. Por ejemplo, el  **&lt;asp: botón&gt;**  control se representa como HTML  **&lt;tipo de entrada =&quot;enviar&quot; &gt;**  elemento.
4. Cierre el explorador.


## <a name="working-with-additional-controls"></a>Trabajar con controles adicionales

<a id="sectionToggle2"></a>

En esta parte del tutorial, trabajará con la [calendario](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) control, que muestra las fechas de un mes a la vez. El [calendario](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) control es un control más complejo que el botón, el cuadro de texto y la etiqueta que ha estado trabajando con e ilustra algunas funciones adicionales de controles de servidor.

En esta sección, agregará un [System.Web.UI.WebControls.Calendar](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) el control a la página y aplíquele el formato adecuado.

### <a name="to-add-a-calendar-control"></a>Para agregar un control de calendario


1. En Visual Studio, cambie a **diseño** vista.
2. Desde el **estándar** sección de la **cuadro de herramientas**, arrastre un [calendario](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) controlar hasta la página y colóquela debajo la **div** elemento que contiene los controles.

    Se muestra el panel de etiquetas inteligentes del calendario. El panel muestra los comandos que facilitan la tarea para realizar las tareas más comunes para el control seleccionado. La siguiente ilustración muestra la [calendario](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) controlar tal y como se representan en **diseño** vista.

    ![Control en la vista Diseño de calendario](creating-a-basic-web-forms-page/_static/image13.png "control en la vista Diseño de calendario")
3. En el panel de etiquetas inteligentes, elija **formato automático**.

    El **formato automático** se muestra el cuadro de diálogo, que permite seleccionar un esquema de formato para el calendario. La siguiente ilustración muestra la **formato automático** cuadro de diálogo para la [calendario](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) control.

    ![Cuadro de diálogo Formato automático (control Calendar)](creating-a-basic-web-forms-page/_static/image14.png "cuadro de diálogo Formato automático (control Calendar)")
4. Desde el **seleccionar un esquema** seleccione **Simple** y, a continuación, haga clic en **Aceptar**.
5. Cambie a **origen** vista.

    Puede ver el  **&lt;asp: Calendar&gt;**  elemento. Este elemento es mucho más larga que los elementos de los controles sencillos que creó anteriormente. También incluye subelementos, como  **&lt;WeekEndDayStyle&gt;**, que representan distintas configuraciones de formato. La siguiente ilustración muestra la [calendario](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) controlar en **origen** vista. (El formato exacto que se ve en **origen** vista puede diferir ligeramente de la ilustración.)

    ![Control de vista del origen de calendario](creating-a-basic-web-forms-page/_static/image15.png "control de vista del origen de calendario")


### <a name="programming-the-calendar-control"></a>Programar el Control de calendario


En esta sección, programará la [calendario](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) control para mostrar la fecha seleccionada actualmente.

### <a name="to-program-the-calendar-control"></a>Para programar el control de calendario


1. En **diseño** ver, haga doble clic en el [calendario](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) control.

    Se crea un nuevo controlador de eventos y se muestra en el archivo de código subyacente denominado *FirstWebPage.aspx.cs*.
2. Finalizar la [Calendar](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.selectionchanged.aspx) controlador de eventos con el código siguiente.


    [!code-csharp[Main](creating-a-basic-web-forms-page/samples/sample3.cs?highlight=3)]


    [!code-vb[Main](creating-a-basic-web-forms-page/samples/sample4.vb?highlight=2)]

 El código anterior, Establece el texto del control label a la fecha seleccionada del control de calendario.


### <a name="running-the-page"></a>Ejecución de la página


Ahora puede probar el calendario.

### <a name="to-run-the-page"></a>Para ejecutar la página


1. Presione **CTRL+F5** para ejecutar la página en el explorador.
2. Haga clic en una fecha del calendario.

    Se muestra la fecha en la que hizo clic en el [etiqueta](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) control.
3. En el explorador, vea el código fuente de la página.

    Tenga en cuenta que la [calendario](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) control se ha procesado en la página como un **tabla**, con cada día como un **td** elemento.
4. Cierre el explorador.


## <a name="next-steps"></a>Pasos siguientes


<a id="nextStepsToggle"></a>

Este tutorial ha mostrado las características básicas del Diseñador de páginas de Visual Studio. Ahora que sabe cómo crear y editar una página de formularios Web Forms en Visual Studio, puede explorar otras características. Por ejemplo, puede hacer lo siguiente:

- Obtener más información sobre ASP.NET Web Forms siguiendo la serie de tutoriales paso a paso [Introducción a formularios Web Forms de ASP.NET 4.5 y Visual Studio 2013](getting-started-with-aspnet-45-web-forms/introduction-and-overview.md).
- Más información acerca de las hojas de estilos en cascada (CSS). Para obtener más información, consulte [Working with CSS Overview](https://msdn.microsoft.com/library/bb398931.aspx).
