---
uid: web-forms/overview/getting-started/code-editing-in-web-forms-pages
title: Edición de código de ASP.NET Web Forms en Visual Studio 2013 | Microsoft Docs
author: Erikre
description: ''
ms.author: riande
ms.date: 03/03/2014
ms.assetid: 5344b74e-b888-479a-92bc-601a33bd61a2
msc.legacyurl: /web-forms/overview/getting-started/code-editing-in-web-forms-pages
msc.type: authoredcontent
ms.openlocfilehash: 5bf10a78b106475ffc01b732b21088bad67eb93d
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836075"
---
<a name="code-editing-aspnet-web-forms-in-visual-studio-2013"></a>Código edición de formularios Web Forms ASP.NET en Visual Studio 2013
====================
por [Erik Reitan](https://github.com/Erikre)

En muchas páginas de formulario Web Forms de ASP.NET, escribir código en Visual Basic, C# u otro lenguaje. El editor de código en Visual Studio puede ayudarle a escribir código rápidamente y ayudarle a evitar errores. Además, el editor proporciona maneras de crear código reutilizable para ayudar a reducir la cantidad de trabajo que necesita hacer.

Este tutorial muestra distintas características del editor de código de Visual Studio.

Durante este tutorial aprenderá a:

- Corrija los errores de codificación de línea.
- Refactorizar y cambiar el nombre de código.
- Cambiar el nombre de las variables y objetos.
- Insertar fragmentos de código.

## <a name="prerequisites"></a>Requisitos previos


Para poder completar este tutorial, necesitará:

- [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) o [Microsoft Visual Studio Express 2013 para Web](https://www.microsoft.com/visualstudio/11/downloads#express-web). .NET Framework se instala automáticamente. 

    > [!NOTE] 
    > 
    > Microsoft Visual Studio 2013 y Microsoft Visual Studio Express 2013 para Web a menudo se hará referencia a que Visual Studio a lo largo de esta serie de tutoriales.  
    >   
    > Si utiliza Visual Studio, en este tutorial se supone que seleccionó el **desarrollo Web** colección de valores de la primera vez que inicia Visual Studio. Para obtener más información, consulte [Cómo: seleccionar configuración de entorno de desarrollo Web](https://msdn.microsoft.com/library/ff521558.aspx).

  Para obtener una introducción a Visual Studio y ASP.NET, vea [crear una página básica de ASP.NET 4.5 Web Forms en Visual Studio 2013](creating-a-basic-web-forms-page.md).   
 

## <a name="creating-a-web-application-project-and-a-page"></a>Creación de un proyecto de aplicación Web y una página

<a id="sectionToggle0"></a>

En esta parte del tutorial, creará un proyecto de aplicación Web y agregue una nueva página a él.

### <a name="to-create-a-web-application-project"></a>Para crear un proyecto de aplicación Web

1. Abra Microsoft Visual Studio.
2. En el menú **Archivo**, seleccione **Nuevo proyecto**.  
    ![Menú archivo](code-editing-in-web-forms-pages/_static/image1.png)

    Aparecerá el cuadro de diálogo **Nuevo proyecto** .
3. Seleccione el **plantillas**  - &gt; **Visual C#**  - &gt; **Web** grupo de plantillas de la izquierda.
4. Elija la **aplicación Web ASP.NET** plantilla en la columna central.
5. Nombre del proyecto ***BasicWebApp*** y haga clic en el **Aceptar** botón.   
![Cuadro de diálogo nuevo proyecto](code-editing-in-web-forms-pages/_static/image2.png)
6. A continuación, seleccione el **formularios Web Forms** plantilla y haga clic en el **Aceptar** botón para crear el proyecto.  
![Cuadro de diálogo nuevo proyecto de ASP.NET](code-editing-in-web-forms-pages/_static/image3.png)  

    Visual Studio crea un nuevo proyecto que incluye funciones predeterminadas según la plantilla de formularios Web Forms.


## <a name="creating-a-new-aspnet-web-forms-page"></a>Crear una nueva página de formularios Web de ASP.NET


Cuando se crea una nueva aplicación de formularios Web Forms mediante el **aplicación Web ASP.NET** plantilla de proyecto, Visual Studio agrega una página ASP.NET (página de formularios Web Forms) denominada *Default.aspx*, así como otros archivos y las carpetas. Puede usar el *Default.aspx* página como página principal de la aplicación Web. Sin embargo, para este tutorial, creará y trabajar con una nueva página.

### <a name="to-add-a-page-to-the-web-application"></a>Para agregar una página a la aplicación Web


1. En **el Explorador de soluciones**, haga clic en el nombre de la aplicación Web (en este tutorial es el nombre de la aplicación **BasicWebSite**) y, a continuación, haga clic en **agregar**  - &gt; **Nuevo elemento**.   
Se abrirá el cuadro de diálogo **Agregar nuevo elemento**.
2. Seleccione el **Visual C#**  - &gt; **Web** grupo de plantillas de la izquierda. A continuación, seleccione **formulario Web Forms** desde la parte central de lista y asígnele el nombre *FirstWebPage.aspx*.   
    ![Agregar cuadro de diálogo nuevo elemento](code-editing-in-web-forms-pages/_static/image4.png)
3. Haga clic en **agregar** para agregar la página de formularios Web Forms al proyecto.  
 Visual Studio crea la nueva página y lo abre.
4. A continuación, establezca esta nueva página como página de inicio predeterminada. En **el Explorador de soluciones**, haga clic en la página nueva denominada *FirstWebPage.aspx* y seleccione **establecer como página principal**. La próxima vez que ejecute esta aplicación para probar nuestro progreso automáticamente verá esta nueva página en el explorador.


## <a name="correcting-inline-coding-errors"></a>Corrección de errores de codificación de Inline


El editor de código en Visual Studio le ayuda a evitar errores como escribir código, y si ha realizado un error, el editor de código le ayuda a corregir el error. En esta parte del tutorial, escribirá una línea de código que ilustran las características de corrección de errores en el editor.

### <a name="to-correct-simple-coding-errors-in-visual-studio"></a>Para corregir los errores de codificación simple en Visual Studio


1. En **diseño** ver, haga doble clic en la página en blanco para crear un controlador para el **carga** eventos de la página.   
   Utiliza el controlador de eventos como un lugar para escribir algo de código.
2. Dentro del controlador, escriba la siguiente línea que contiene un error y presione **ENTRAR**:

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample1.cs)]

   Al presionar **ENTRAR**, el editor de código coloca un subrayado rojo y verde (llaman habitualmente a &quot;ondulada&quot; líneas) en las áreas del código que tienen problemas. Un subrayado de color verde indica una advertencia. Un subrayado rojo indica un error que debe corregir. 

    Mantenga el puntero del mouse sobre `myStr` para ver información sobre herramientas que muestra información acerca de la advertencia. Además, mantenga el puntero del mouse sobre el subrayado rojo para ver el mensaje de error.

    La siguiente imagen muestra el código con los subrayados.

    ![Texto de bienvenida en la vista Diseño](code-editing-in-web-forms-pages/_static/image5.png "texto de bienvenida en la vista Diseño")  
   El error debe corregirse mediante la adición de un punto y coma `;` al final de la línea. La advertencia simplemente le notifica que no ha usado el `myStr` variable todavía.  

    > [!NOTE] 
    > 
    > Ver el código actual, dar formato a la configuración de Visual Studio seleccionando **herramientas**  - &gt; **opciones**  - &gt; **fuentes y Colores**.


## <a name="refactoring-and-renaming"></a>Refactorización y cambiar el nombre

La refactorización es una metodología de software que implica la reestructuración del código para que sea más fácil de entender y mantener, conservando su funcionalidad. Podría ser un ejemplo sencillo que escribir código en un controlador de eventos para obtener datos de una base de datos. Al desarrollar la página, que detecte que son necesarias tener acceso a los datos de varios controladores diferentes. Por lo tanto, refactorizar el código de la página mediante la creación de un método de acceso a datos en la página y la inserción de llamadas al método en los controladores.

El editor de código incluye herramientas para ayudarle a realizar diversas tareas de refactorización. En este tutorial, trabajará con dos técnicas de refactorización: cambiar el nombre de las variables y extracción de métodos. Otras opciones de refactorización incluyen la encapsulación de campos, promoción de las variables locales a parámetros de método y administración de los parámetros de método. La disponibilidad de estas opciones de refactorización depende de la ubicación en el código.

### <a name="refactoring-code"></a>Refactorización de código

Un escenario común de refactorización consiste en crear (extraer) un método desde el código que está dentro de otro miembro, como un método. Esto reduce el tamaño del miembro original y hace que el código extraído reutilizables.

En esta parte del tutorial, escribirá cierto código simple y, a continuación, extraer un método de él. Se admite la refactorización de C#, por lo que creará una página que usa C# como lenguaje de programación.

### <a name="to-extract-a-method-in-a-c-page"></a>Para extraer un método en una página de C#

1. Cambie a **diseño** vista.
2. En el **cuadro de herramientas**, desde el **estándar** pestaña, arrastre un [botón](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) en la página.
3. Haga doble clic en el **botón** control para crear un controlador para su [haga clic en](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) eventos y, a continuación, agregue el código resaltado siguiente:

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample2.cs?highlight=3-16)]

   El código crea un **ArrayList** objeto, usa un bucle para cargarlo con valores y, a continuación, utiliza otro bucle para mostrar el contenido de la **ArrayList** objeto.
4. Presione **CTRL+F5** para ejecutar la página y, a continuación, haga clic en el **botón** para asegurarse de que ve el siguiente resultado:   

    [!code-html[Main](code-editing-in-web-forms-pages/samples/sample3.html)]
5. Vuelva al editor de código y, a continuación, seleccione las líneas siguientes en el controlador de eventos.   

    [!code-html[Main](code-editing-in-web-forms-pages/samples/sample4.html)]
6. Haga clic en la selección, haga clic en **refactorizar**y, a continuación, elija **Extraer método**. 

    El **Extraer método** aparece el cuadro de diálogo.
7. En el **nombre del nuevo método** , escriba **DisplayArray**y, a continuación, haga clic en **Aceptar**. 

    El editor de código crea un nuevo método denominado `DisplayArray`y coloca una llamada al método nuevo en el **haga clic en** controlador donde estaba originalmente el bucle.

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample5.cs?highlight=12)]
8. Presione **CTRL+F5** volver a ejecutar la página y haga clic en el **botón**.

    La página funciona igual como lo hacía anteriormente. El `DisplayArray` método ahora puede ser llamada desde cualquier lugar en la clase de página.

## <a name="renaming-variables"></a>Cambiar el nombre de Variables

Al trabajar con variables, así como los objetos, puede cambiar su nombre una vez que haya hecho referencia en el código. Sin embargo, al cambiar el nombre de las variables y objetos puede provocar la ruptura si se salta al cambiar el nombre de una de las referencias del código. Por lo tanto, puede utilizar la refactorización para realizar el cambio de nombre.

### <a name="to-use-refactoring-to-rename-a-variable"></a>Para utilizar la refactorización para cambiar el nombre de una variable


1. En el **haga clic en** controlador de eventos, busque la línea siguiente:

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample6.cs)]
2. Haga clic en el nombre de variable `alist`, elija **refactorizar**y, a continuación, elija **cambiar el nombre**.

    El **cambiar el nombre** aparece el cuadro de diálogo.
3. En el **nuevo nombre** , escriba **ArrayList1** y asegúrese de que el **vista previa de cambios de referencia** se ha seleccionado la casilla de verificación. A continuación, haga clic en **Aceptar**.

    El **vista previa de cambios** cuadro de diálogo aparece y muestra un árbol que contiene todas las referencias a la variable que ha cambiado.
4. Haga clic en **aplicar** para cerrar el **vista previa de cambios** cuadro de diálogo.

    Se cambia el nombre de las variables que se refieren específicamente a la instancia que ha seleccionado. Sin embargo, tenga en cuenta que la variable `alist` en la siguiente línea no es cambiar el nombre.

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample7.cs)]

    La variable `alist` en esta línea no es cambiar el nombre porque no representa el mismo valor que la variable `alist` cuyo nombre ha cambiado. La variable `alist` en el `DisplayArray` declaración es una variable local para ese método. Esto ilustra que mediante la refactorización para cambiar el nombre de variables es diferente que simplemente realizar una acción de buscar y reemplazar en el editor; refactorización variables cambia el nombre con el conocimiento de la semántica de la variable que se está trabajando.


## <a name="inserting-snippets"></a>Insertar fragmentos de código

Dado que hay muchas tareas de codificación que los desarrolladores de formularios Web Forms con frecuencia necesitan realizar, el editor de código proporciona una biblioteca de fragmentos de código o bloques de código predefinido. Puede insertar estos fragmentos de código en la página.

Todos los idiomas que se usan en Visual Studio tiene ligeras diferencias en la manera de que insertar fragmentos de código. Para obtener información acerca de cómo insertar fragmentos de código, vea [fragmentos de código de IntelliSense de Visual Basic](https://msdn.microsoft.com/library/18yz4be4.aspx). Para obtener información acerca de cómo insertar fragmentos de código en Visual C#, vea [fragmentos de código de Visual C#](https://msdn.microsoft.com/library/z41h7fat.aspx).

## <a name="next-steps"></a>Pasos siguientes

Este tutorial ha mostrado las características básicas del editor de código de Visual Studio 2010 para corregir errores en el código, refactorizar el código, cambiar el nombre de las variables e insertar fragmentos de código en el código. Características adicionales en el editor pueden realizar el desarrollo de aplicaciones rápido y sencillo. Por ejemplo, puedes:

- Obtenga más información acerca de las características de IntelliSense, como modificar opciones de IntelliSense, administración de fragmentos de código y buscar fragmentos de código en línea. Para obtener más información, vea [Usar IntelliSense](https://msdn.microsoft.com/library/hcw1s69b.aspx).
- Obtenga información sobre cómo crear sus propios fragmentos de código. Para obtener más información, consulte [creación y uso de fragmentos de código de IntelliSense](https://msdn.microsoft.com/library/ms165392.aspx)
- Más información sobre las características específicas de Visual Basic de fragmentos de código de IntelliSense, como personalizar los fragmentos de código y solución de problemas. Para obtener más información, consulte [fragmentos de código de IntelliSense de Visual Basic](https://msdn.microsoft.com/library/18yz4be4.aspx)
- Más información sobre C#-características específicas de IntelliSense, como la refactorización y fragmentos de código. Para obtener más información, consulte [Visual C# IntelliSense](https://msdn.microsoft.com/library/43f44291.aspx).
