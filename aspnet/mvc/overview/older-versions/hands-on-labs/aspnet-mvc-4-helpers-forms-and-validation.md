---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
title: Las aplicaciones auxiliares de ASP.NET MVC 4, formularios y la validación | Microsoft Docs
author: rick-anderson
description: En los modelos de ASP.NET MVC 4 y laboratorios prácticos de acceso de datos, ha estado cargar y mostrar los datos de la base de datos. En este laboratorio práctico, agregará a la...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 187ee9cd-bc70-479b-bfed-f568b8da96eb
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
msc.type: authoredcontent
ms.openlocfilehash: 8671ae8e9408e6f05135fa27d56480477521c4ba
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829461"
---
# <a name="aspnet-mvc-4-helpers-forms-and-validation"></a>Validación, formularios y aplicaciones auxiliares de ASP.NET MVC 4

por [campamentos Web Team](https://twitter.com/webcamps)

[Descargue el Kit de aprendizaje de campamentos de Web](https://aka.ms/webcamps-training-kit)

En **acceso a datos y modelos de ASP.NET MVC 4** laboratorios prácticos, han sido cargar y mostrar los datos de la base de datos. En este laboratorio práctico, agregará el **Music Store** la capacidad de editar los datos de la aplicación.

Con ese objetivo en mente, creará primero el controlador que será compatible con las acciones de creación, lectura, actualización y eliminación (CRUD) de álbumes. Se generará una plantilla de vista de índice que aprovecha la característica de scaffolding de ASP.NET MVC para mostrar las propiedades de los álbumes en una tabla HTML. Para mejorar esa vista, deberá agregar una aplicación auxiliar HTML personalizada que se truncará descripciones largas.

Después, agregará la edición y crear vistas que se pueden modificar los álbumes en la base de datos, con la Ayuda de los elementos de formulario como listas desplegables.

Por último, permitirá a los usuarios eliminar un álbum y también se les impedirá introducir datos incorrectos al validar su entrada.

Esta práctica se da por supuesto que tiene conocimientos básicos de **ASP.NET MVC**. Si no ha usado **ASP.NET MVC** antes, le recomendamos que repase **aspectos básicos de ASP.NET MVC** laboratorios prácticos.

Esta práctica le guiará a través de las mejoras y nuevas características descritas anteriormente aplicando cambios menores a una aplicación Web de ejemplo proporcionada en la carpeta de origen.

> [!NOTE]
> Todo el código de ejemplo y fragmentos de código se incluyen en el Kit de entrenamiento campamentos de Web, que está disponible en [versiones de Microsoft de Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit). Está disponible en el proyecto específico para este laboratorio [las aplicaciones auxiliares de ASP.NET MVC 4, formularios y la validación](https://github.com/Microsoft-Web/HOL-MVC4HelpersFormsAndValidation).

<a id="Objectives"></a>
### <a name="objectives"></a>Objetivos

En este laboratorio práctico, aprenderá cómo:

- Crear un controlador para que admita operaciones CRUD
- Generar una vista de índice para mostrar las propiedades de entidad en una tabla HTML
- Agregar una aplicación auxiliar HTML personalizada
- Crear y personalizar una vista de edición
- Diferenciar entre los métodos de acción que reaccionan a HTTP-GET o las llamadas HTTP-POST
- Agregar y personalizar una instrucción Create View
- Identificador de la eliminación de una entidad
- Validar entrada de usuario

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Requisitos previos

Debe tener los elementos siguientes para completar esta práctica:

- [Microsoft Visual Studio Express 2012 para Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) o superior (leer [Apéndice A](#AppendixA) para obtener instrucciones sobre cómo instalarlo).

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a>Programa de instalación

**Instalación de fragmentos de código**

Para mayor comodidad, gran parte del código que se va a administrar a lo largo de este laboratorio está disponible como fragmentos de código de Visual Studio. Para instalar los fragmentos de código que se ejecute **.\Source\Setup\CodeSnippets.vsi** archivo.

Si no está familiarizado con los fragmentos de código de Visual Studio y desea aprender a usarlas, puede consultar el apéndice de este documento &quot; [Apéndice B: Using Code Snippets](#AppendixB)&quot;.

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Ejercicios

Los ejercicios siguientes constituyen este laboratorio práctico:

1. [Crear el controlador de Store Manager y su vista de índice](#Exercise1)
2. [Agregar una aplicación auxiliar HTML](#Exercise2)
3. [Creación de la vista de edición](#Exercise3)
4. [Agregar una vista Create](#Exercise4)
5. [Eliminación de control](#Exercise5)
6. [Agregar una validación](#Exercise6)
7. [Uso de jQuery discreta en el lado cliente](#Exercise7)

> [!NOTE]
> Cada ejercicio está acompañado por un **final** carpeta que contiene la solución resultante debe obtener después de completar los ejercicios. Puede usar esta solución como una guía si necesita ayuda adicional para trabajar a través de los ejercicios.


Tiempo estimado para completar esta práctica: **60 minutos**

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_the_Store_Manager_controller_and_its_Index_view"></a>
### <a name="exercise-1-creating-the-store-manager-controller-and-its-index-view"></a>Ejercicio 1: Crear el controlador de Store Manager y su vista de índice

En este ejercicio, obtendrá información sobre cómo crear un nuevo controlador para admitir operaciones CRUD, personalizar su método de acción de índice para devolver una lista de álbumes de la base de datos y, por último, generar una plantilla de vista de índice que aprovecha el scaffolding de ASP.NET MVC función para mostrar las propiedades de los álbumes en una tabla HTML.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_StoreManagerController"></a>
#### <a name="task-1---creating-the-storemanagercontroller"></a>Tarea 1: crear el StoreManagerController

En esta tarea, creará un nuevo controlador denominado **StoreManagerController** para admitir las operaciones CRUD.

1. Abra el **comenzar** solución ubicado en **origen/Ex1-CreatingTheStoreManagerController/inicio/** carpeta.

   1. Deberá descargar algunos paquetes de NuGet que faltan antes de continuar. Para ello, haga clic en el **proyecto** menú y seleccione **administrar paquetes de NuGet**.
   2. En el **administrar paquetes de NuGet** cuadro de diálogo, haga clic en **restaurar** para descargar los paquetes que falten.
   3. Por último, compile la solución haciendo **compilar** | **compilar solución**.

      > [!NOTE]
      > Una de las ventajas de usar NuGet es que no tienen que enviar todas las bibliotecas en el proyecto, lo que reduce el tamaño del proyecto. Con las herramientas avanzadas de NuGet, mediante la especificación de las versiones del paquete en el archivo Packages.config, podrá descargar todas las bibliotecas requeridas en la primera vez que ejecute el proyecto. Esto es por eso tendrá que ejecutar estos pasos después de abrir una solución existente de este laboratorio.
2. Agregar un nuevo controlador. Para ello, haga clic en el **controladores** carpeta en el Explorador de soluciones, seleccione **agregar** y, a continuación, el **controlador** comando. Cambiar el **controlador** **nombre** a **StoreManagerController** y asegúrese de que la opción **controlador de MVC con acciones de lectura/escritura vacías**está seleccionada. Haga clic en **Agregar**.

    ![Cuadro de diálogo Agregar controlador](aspnet-mvc-4-helpers-forms-and-validation/_static/image1.png "cuadro de diálogo Agregar controlador")

    *Agregar cuadro de diálogo de controlador*

    Se genera una nueva clase de controlador. Puesto que indican para agregar acciones de lectura/escritura, los métodos auxiliares para los usuarios, acciones comunes de CRUD se crean con comentarios TODO rellenados, preguntar para incluir la lógica específica de la aplicación.

<a id="Ex1Task2"></a>

<a id="Task_2_-_Customizing_the_StoreManager_Index"></a>
#### <a name="task-2---customizing-the-storemanager-index"></a>Tarea 2: personalizar el índice StoreManager

En esta tarea, personalizará el método de acción del índice StoreManager para devolver una vista con la lista de álbumes de la base de datos.

1. En la clase StoreManagerController, agregue la siguiente *mediante* directivas.

    (Código de fragmento de código - *las aplicaciones auxiliares de ASP.NET MVC 4 y formularios y validación: Ex1 mediante MvcMusicStore*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample1.cs)]
2. Agregar un campo a la **StoreManagerController** para contener una instancia de **MusicStoreEntities.**

    (Código de fragmento de código - *aplicaciones auxiliares de ASP.NET MVC 4 y formularios y validación: Ex1 MusicStoreEntities*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample2.cs)]
3. Implementar la acción del índice de StoreManagerController para devolver una vista con la lista de álbumes.

    La lógica de la acción de controlador será muy similar a la acción del índice del StoreController escrito anteriormente. Usar LINQ para recuperar todos los álbumes, incluida la información de género y el intérprete para su presentación.

    (Código de fragmento de código - *aplicaciones auxiliares de ASP.NET MVC 4 y formularios y validación: Ex1 StoreManagerController índice*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample3.cs)]

<a id="Ex1Task3"></a>

<a id="strongTask_3_-_Creating_the_Index_Viewstrong"></a>
#### <a name="task-3---creating-the-index-view"></a>Tarea 3: creación de la vista de índice

En esta tarea, creará la plantilla de vista de índice para mostrar la lista de álbumes devuelto por la **StoreManager** controlador.

1. Antes de crear la nueva plantilla de vista, debe compilar el proyecto para que el **Agregar cuadro de diálogo de vista** conoce la **álbum** clase se debe utilizar. Seleccione **compilar | Compilar MvcMusicStore** para compilar el proyecto.
2. Haga clic en el **índice** método de acción y seleccione **agregar vista**. Esto le llevará a la **agregar vista** cuadro de diálogo.

    ![Agregar vista](aspnet-mvc-4-helpers-forms-and-validation/_static/image2.png "agregar vista")

    *Agregar una vista desde dentro del método de índice*
3. En el cuadro de diálogo Agregar vista, compruebe que el nombre de vista es **índice**. Seleccione el **crear una vista fuertemente tipada** opción y seleccione **álbum (MvcMusicStore.Models)** desde el **clase modelo** lista desplegable. Seleccione **lista** desde el **plantilla Scaffold** lista desplegable. Deje el **motor de vista** a **Razor** y los demás campos con su valor predeterminado de valor y, a continuación, haga clic en **agregar**.

    ![Agregar una vista de índice](aspnet-mvc-4-helpers-forms-and-validation/_static/image3.png "agregar una vista de índice")

    *Agregar una vista de índice*

<a id="Ex1Task4"></a>

<a id="Task_4_-_Customizing_the_scaffold_of_the_Index_View"></a>
#### <a name="task-4---customizing-the-scaffold-of-the-index-view"></a>Tarea 4: personalizar las plantillas scaffold de la vista de índice

En esta tarea, se ajustará la plantilla de vista simple creada con la característica de scaffolding de ASP.NET MVC para que muestre los campos que desee.

> [!NOTE]
> El **scaffolding** compatibilidad dentro de ASP.NET MVC genera una plantilla de vista simple que enumera todos los campos en el modelo de álbum. **Scaffolding** proporciona una forma rápida de empezar a trabajar en una vista fuertemente tipada: en lugar de tener que escribir manualmente la plantilla de vista, scaffolding rápidamente genera una plantilla predeterminada y, a continuación, puede modificar el código generado.


1. Revise el código creado. La lista de campos generada formarán parte de los siguientes valores de tabla HTML que **Scaffolding** usa para mostrar datos tabulares.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample4.cshtml)]
2. Reemplace el **&lt;tabla&gt;** código con el código siguiente para mostrar solamente el **género**, **artista**, **título del álbum**, y **precio** campos. Esto elimina la **AlbumId** y **URL carátulas de álbum** columnas. Además, cambia GenreId y ArtistId columnas para mostrar sus propiedades de la clase vinculado del **Artist.Name** y **Genre.Name**y quita el **detalles** vínculo.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample5.cshtml)]
3. Cambie las siguientes descripciones.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample6.cshtml)]

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Tarea 5: ejecución de la aplicación

En esta tarea, probará que la **StoreManager** **índice** plantilla de vista muestra una lista de álbumes de acuerdo con el diseño de los pasos anteriores.

1. Presione **F5** para ejecutar la aplicación.
2. El proyecto se inicia en la página principal. Cambiar la dirección URL de **/StoreManager** para comprobar que se muestra una lista de álbumes, que muestra su **título**, **artista** y **género**.

    ![Examinar la lista de álbumes](aspnet-mvc-4-helpers-forms-and-validation/_static/image4.png "examinar la lista de álbumes")

    *Examinar la lista de álbumes*

<a id="Exercise2"></a>

<a id="Exercise_2_Adding_an_HTML_Helper"></a>
### <a name="exercise-2-adding-an-html-helper"></a>Ejercicio 2: Agregar una aplicación auxiliar HTML

La página de índice StoreManager tiene un problema potencial: propiedades de título y el nombre del intérprete tanto pueden ser lo suficientemente largo para deshacerse del formato de tabla. En este ejercicio obtendrá información sobre cómo agregar una aplicación auxiliar HTML personalizada para truncar ese texto.

En la ilustración siguiente, puede ver cómo se modifica el formato debido a la longitud del texto cuando se usa un tamaño pequeño de explorador.

![Examinar la lista de álbumes con texto truncado no](aspnet-mvc-4-helpers-forms-and-validation/_static/image5.png "no examinar la lista de álbumes con texto truncado")

*No examinar la lista de álbumes con texto truncado*

<a id="Ex2Task1"></a>

<a id="Task_1_-_Extending_the_HTML_Helper"></a>
#### <a name="task-1---extending-the-html-helper"></a>Tarea 1: Extender la aplicación auxiliar HTML

En esta tarea, agregará un nuevo método **Truncate** a la **HTML** objeto expuesto en las vistas de MVC de ASP.NET. Para ello, implementará un **método de extensión** a la integrada **System.Web.Mvc.HtmlHelper** clase proporcionada por ASP.NET MVC.

> [!NOTE]
> Para obtener más información acerca de **métodos de extensión**, visite este artículo de msdn. [https://msdn.microsoft.com/library/bb383977.aspx](https://msdn.microsoft.com/library/bb383977.aspx).


1. Abra el **comenzar** solución ubicado en **origen/Ex2-AddingAnHTMLHelper/inicio/** carpeta. En caso contrario, es posible que siga usando la **final** solución obtenido completando el ejercicio anterior.

   1. Si abrió proporcionado **comenzar** solución, deberá descargar algunos paquetes de NuGet que faltan antes de continuar. Para ello, haga clic en el **proyecto** menú y seleccione **administrar paquetes de NuGet**.
   2. En el **administrar paquetes de NuGet** cuadro de diálogo, haga clic en **restaurar** para descargar los paquetes que falten.
   3. Por último, compile la solución haciendo **compilar** | **compilar solución**.

      > [!NOTE]
      > Una de las ventajas de usar NuGet es que no tienen que enviar todas las bibliotecas en el proyecto, lo que reduce el tamaño del proyecto. Con las herramientas avanzadas de NuGet, mediante la especificación de las versiones del paquete en el archivo Packages.config, podrá descargar todas las bibliotecas requeridas en la primera vez que ejecute el proyecto. Esto es por eso tendrá que ejecutar estos pasos después de abrir una solución existente de este laboratorio.
2. Abra la vista de índice del StoreManager. Para ello, en el Explorador de soluciones, expanda el **vistas** carpeta, el **StoreManager** y abra el **Index.cshtml** archivo.
3. Agregue el código siguiente el <strong>@model</strong> directiva para definir el <strong>Truncate</strong> método auxiliar.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample7.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Truncating_Text_in_the_Page"></a>
#### <a name="task-2---truncating-text-in-the-page"></a>Tarea 2 - truncar el texto de la página

En esta tarea, utilizará el **Truncate** método truncar el texto en la plantilla de vista.

1. Abra la vista de índice del StoreManager. Para ello, en el Explorador de soluciones, expanda el **vistas** carpeta, el **StoreManager** y abra el **Index.cshtml** archivo.
2. Reemplace las líneas que muestran la **el nombre del intérprete** y del álbum **título**. Para ello, reemplace las líneas siguientes.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample8.cshtml)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Tarea 3: ejecutar la aplicación

En esta tarea, probará que la **StoreManager** **índice** ver plantilla trunca el título y el nombre del intérprete del álbum.

1. Presione **F5** para ejecutar la aplicación.
2. El proyecto se inicia en la página principal. Cambiar la dirección URL de **/StoreManager** para comprobar ese tiempo textos en el **título** y **artista** columna se truncan.

    ![Trunca los nombres de los títulos y los artistas](aspnet-mvc-4-helpers-forms-and-validation/_static/image6.png "trunca los nombres de los títulos y los artistas")

    *Nombres de intérpretes y títulos truncados*

<a id="Exercise3"></a>

<a id="Exercise_3_Creating_the_Edit_View"></a>
### <a name="exercise-3-creating-the-edit-view"></a>Ejercicio 3: Creación de la vista de edición

En este ejercicio, obtendrá información sobre cómo crear un formulario para permitir que los responsables de tienda editar un álbum. Examinará el **/StoreManager/Edit/id** URL (**id** está el identificador único del álbum que desea editar), lo que hace que una llamada HTTP-GET en el servidor.

El método de acción Edit de controlador se recupere el álbum adecuado de la base de datos, cree un **StoreManagerViewModel** objeto encapsularlo (junto con una lista de los artistas y géneros) y, a continuación, pasarlos para una plantilla de vista representar la página HTML al usuario. Esta página contiene un **&lt;formulario&gt;** elemento con los cuadros de texto y listas desplegables para editar las propiedades del álbum.

Una vez que el usuario actualiza los valores de formulario de álbum y hace clic en el **guardar** botón, los cambios se envían a través de devolución de llamada HTTP-POST a **/StoreManager/Edit/id**. Aunque la dirección URL sigue siendo el mismo que en la última llamada, ASP.NET MVC que identifica este tiempo es una solicitud HTTP-POST y, por tanto, se ejecuta un método de acción Edit distintos (uno decorado con **[HttpPost]**).

<a id="Ex3Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Edit_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-edit-action-method"></a>Tarea 1: implementar el método de acción Edit de HTTP-GET

En esta tarea, implementará la versión de HTTP-GET del método de acción de edición para recuperar el álbum adecuado de la base de datos, así como una lista de todos los géneros y artistas. Empaquetará estos datos de en el **StoreManagerViewModel** definido en el último paso, que, a continuación, se pasará a una plantilla de vista para representar la respuesta con el objeto.

1. Abra el **comenzar** solución ubicado en **origen/Ex3-CreatingTheEditView/inicio/** carpeta. En caso contrario, es posible que siga usando la **final** solución obtenido completando el ejercicio anterior.

   1. Si abrió proporcionado **comenzar** solución, deberá descargar algunos paquetes de NuGet que faltan antes de continuar. Para ello, haga clic en el **proyecto** menú y seleccione **administrar paquetes de NuGet**.
   2. En el **administrar paquetes de NuGet** cuadro de diálogo, haga clic en **restaurar** para descargar los paquetes que falten.
   3. Por último, compile la solución haciendo **compilar** | **compilar solución**.

      > [!NOTE]
      > Una de las ventajas de usar NuGet es que no tienen que enviar todas las bibliotecas en el proyecto, lo que reduce el tamaño del proyecto. Con las herramientas avanzadas de NuGet, mediante la especificación de las versiones del paquete en el archivo Packages.config, podrá descargar todas las bibliotecas requeridas en la primera vez que ejecute el proyecto. Esto es por eso tendrá que ejecutar estos pasos después de abrir una solución existente de este laboratorio.
2. Abra el **StoreManagerController** clase. Para ello, expanda el **controladores** carpeta y haga doble clic en **StoreManagerController.cs**.
3. Reemplace el **HTTP-GET editar** método de acción con el código siguiente para recuperar la correspondiente **álbum** , así como la **géneros** y **artistas**enumera.

    (Código de fragmento de código - *las aplicaciones auxiliares de ASP.NET MVC 4 y formularios y validación: Ex3 StoreManagerController HTTP-GET Editar acción*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample9.cs)]

    > [!NOTE]
    > Usa **System.Web.Mvc** **SelectList** para artistas y géneros en lugar de la **System.Collections.Generic** lista.
    > 
    > **SelectList** es una forma más limpia para rellenar las listas desplegables HTML y administran cosas como la selección actual. Creación de instancias y configuración posterior de estos objetos de ViewModel en la acción de controlador hará que el escenario de formulario de edición más limpio.

<a id="Ex3Task2"></a>

<a id="Task_2_-_Creating_the_Edit_View"></a>
#### <a name="task-2---creating-the-edit-view"></a>Tarea 2: crear la vista de edición

En esta tarea, creará una plantilla de vista de edición que más adelante se mostrará las propiedades del álbum.

1. Crear la vista de edición. Para ello, haga clic en el **editar** método de acción y seleccione **agregar vista**.
2. En el cuadro de diálogo Agregar vista, compruebe que el nombre de vista es **editar**. Compruebe el **crear una vista fuertemente tipada** casilla y seleccione **álbum (MvcMusicStore.Models)** desde el **Ver clase de datos** lista desplegable. Seleccione **editar** desde el **plantilla Scaffold** lista desplegable. Deje los demás campos con sus valores predeterminados y, a continuación, haga clic en **agregar**.

    ![Agregar una vista de edición](aspnet-mvc-4-helpers-forms-and-validation/_static/image7.png "agregar una vista de edición")

    *Agregar una vista de edición*

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Tarea 3: ejecutar la aplicación

En esta tarea, probará que la **StoreManager** **editar** página de vista muestra los valores de las propiedades del álbum pasado como parámetro.

1. Presione **F5** para ejecutar la aplicación.
2. El proyecto se inicia en la página principal. Cambiar la dirección URL de **/StoreManager/Edit/1** para comprobar que se muestran los valores de las propiedades del álbum pasado.

    ![Exploración de la vista de edición del álbum](aspnet-mvc-4-helpers-forms-and-validation/_static/image8.png "exploración Editar vista del álbum")

    *Exploración de la vista de edición del álbum*

<a id="Ex3Task4"></a>

<a id="Task_4_-_Implementing_drop-downs_on_the_Album_Editor_Template"></a>
#### <a name="task-4---implementing-drop-downs-on-the-album-editor-template"></a>Tarea 4: implementación de listas desplegables en la plantilla del Editor de álbum

En esta tarea, agregará listas desplegables a la plantilla de vista que se creó en la última tarea, para que el usuario puede seleccionar entre una lista de los artistas y géneros.

1. Reemplazar todo el **álbum** fieldset código con lo siguiente:

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample10.cshtml)]

    > [!NOTE]
    > Un **Html.DropDownList** auxiliar se agregó para representar listas desplegables para elegir los artistas y géneros. Los parámetros pasados a **Html.DropDownList** son:
    > 
    > 1. El nombre del campo de formulario (**&quot;ArtistId&quot;**).
    > 2. El **SelectList** de valores de la lista desplegable.

<a id="Ex3Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Tarea 5: ejecución de la aplicación

En esta tarea, probará que la **StoreManager** **editar** ver página muestra listas desplegables en lugar de campos de texto del artista y el Id. de género.

1. Presione **F5** para ejecutar la aplicación.
2. El proyecto se inicia en la página principal. Cambiar la dirección URL de **/StoreManager/Edit/1** para comprobar que muestra en lugar de campos de texto del artista y el Id. de género listas desplegables.

    ![Exploración de la vista de edición del álbum con listas desplegables](aspnet-mvc-4-helpers-forms-and-validation/_static/image9.png "vista de edición del álbum de exploración con listas desplegables")

    *Exploración de la vista de edición del álbum, esta vez con listas desplegables*

<a id="Ex3Task6"></a>

<a id="Task_6_-_Implementing_the_HTTP-POST_Edit_action_method"></a>
#### <a name="task-6---implementing-the-http-post-edit-action-method"></a>Tarea 6: implementar el método de acción Edit de HTTP-POST

Ahora que la vista de edición se muestra según lo esperado, deberá implementar el método de acción de editar HTTP-POST para guardar los cambios realizados en el álbum.

1. Cierre el explorador si es necesario, para volver a la ventana de Visual Studio. Abra **StoreManagerController** desde el **controladores** carpeta.
2. Reemplace **HTTP-POST editar** código del método de acción con los siguientes (tenga en cuenta que el método que se debe reemplazar es una versión sobrecargada que recibe dos parámetros):

    (Código de fragmento de código - *las aplicaciones auxiliares de ASP.NET MVC 4 y formularios y validación: Ex3 StoreManagerController HTTP-POST Editar acción*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample11.cs)]

    > [!NOTE]
    > Este método se ejecutará cuando el usuario hace clic en el **guardar** botón de la vista y realiza una solicitud HTTP-POST de los valores de formulario al servidor para hacer que persistan en la base de datos. El elemento decorator **[HttpPost]** indica que el método debe usarse para los escenarios de HTTP-POST. El método toma un **álbum** objeto. ASP.NET MVC creará automáticamente el objeto de álbum desde la registrada &lt;formulario&gt; valores.
    > 
    > El método llevará a cabo estos pasos:
    > 
    > 1. Si el modelo es válido:
    > 
    >     1. Actualice la entrada de álbum en el contexto para marcarla como un objeto modificado.
    >     2. Guarde los cambios y redirigir a la vista de índice.
    > 2. Si el modelo no es válido, rellenará con ViewBag el **GenreId** y **ArtistId**, devolverá la vista con el objeto de álbum recibido para permitir al usuario realizar cualquier actualización necesaria.

<a id="Ex3Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a>Tarea 7: ejecución de la aplicación

En esta tarea, probará que la **StoreManager edición** página ver realmente guarda los datos actualizados del álbum en la base de datos.

1. Presione **F5** para ejecutar la aplicación.
2. El proyecto se inicia en la página principal. Cambiar la dirección URL de **/StoreManager/Edit/1**. Cambie el título de álbum a **carga** y haga clic en **guardar**. Compruebe que el título del álbum cambiado realmente en la lista de álbumes.

    ![Actualizar un álbum](aspnet-mvc-4-helpers-forms-and-validation/_static/image10.png "actualizando un álbum")

    *Actualización de un álbum*

<a id="Exercise4"></a>

<a id="Exercise_4_Adding_a_Create_View"></a>
### <a name="exercise-4-adding-a-create-view"></a>Ejercicio 4: Agregar una vista de creación

Ahora que la **StoreManagerController** admite el **editar** capacidad, en este ejercicio obtendrá información sobre cómo agregar una plantilla Create View para que pueda almacenar los administradores agregarán álbumes nuevos a la aplicación.

Igual que con la funcionalidad de edición, se implementará el escenario de creación mediante dos métodos independientes dentro de la **StoreManagerController** clase:

1. Un método de acción mostrará un formulario vacío cuando visite por primera vez los responsables de tienda el **/StoreManager/crear** dirección URL.
2. Un segundo método de acción controlará el escenario donde hace clic en el administrador del almacén la **guardar** situado dentro del formulario y envía los valores de nuevo a la **/StoreManager/crear** dirección URL como una solicitud HTTP-POST.

<a id="Ex4Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Create_action_method"></a>
#### <a name="task-1---implementing-the-http-get-create-action-method"></a>Tarea 1: implementar el método de acción Create HTTP-GET

En esta tarea, implementará la versión de HTTP-GET del método de acción Create para recuperar una lista de todos los géneros e intérpretes, empaquete estos datos en un **StoreManagerViewModel** objeto, que, a continuación, se pasará a una plantilla de vista.

1. Abra el **comenzar** solución ubicado en **origen/Ex4-AddingACreateView/inicio/** carpeta. En caso contrario, es posible que siga usando la **final** solución obtenido completando el ejercicio anterior.

   1. Si abrió proporcionado **comenzar** solución, deberá descargar algunos paquetes de NuGet que faltan antes de continuar. Para ello, haga clic en el **proyecto** menú y seleccione **administrar paquetes de NuGet**.
   2. En el **administrar paquetes de NuGet** cuadro de diálogo, haga clic en **restaurar** para descargar los paquetes que falten.
   3. Por último, compile la solución haciendo **compilar** | **compilar solución**.

      > [!NOTE]
      > Una de las ventajas de usar NuGet es que no tienen que enviar todas las bibliotecas en el proyecto, lo que reduce el tamaño del proyecto. Con las herramientas avanzadas de NuGet, mediante la especificación de las versiones del paquete en el archivo Packages.config, podrá descargar todas las bibliotecas requeridas en la primera vez que ejecute el proyecto. Esto es por eso tendrá que ejecutar estos pasos después de abrir una solución existente de este laboratorio.
2. Abra **StoreManagerController** clase. Para ello, expanda el **controladores** carpeta y haga doble clic en **StoreManagerController.cs**.
3. Reemplace el **crear** código del método de acción con lo siguiente:

    (Código de fragmento de código - *las aplicaciones auxiliares de ASP.NET MVC 4 y formularios y validación: acción Ex4 StoreManagerController HTTP-GET crear*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample12.cs)]

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_the_Create_View"></a>
#### <a name="task-2---adding-the-create-view"></a>Tarea 2: adición de la vista de creación

En esta tarea, agregará la plantilla Create View que se mostrará un nuevo formulario de álbum (vacío).

1. Haga clic en el **crear** método de acción y seleccione **agregar vista**. Se abrirá el cuadro de diálogo Agregar vista.
2. En el cuadro de diálogo Agregar vista, compruebe que el nombre de vista es **crear**. Seleccione el **crear una vista fuertemente tipada** opción y seleccione **álbum (MvcMusicStore.Models)** desde el **clase modelo** desplegable y **crear** desde el **plantilla Scaffold** lista desplegable. Deje los demás campos con sus valores predeterminados y, a continuación, haga clic en **agregar**.

    ![Agregar una vista create](aspnet-mvc-4-helpers-forms-and-validation/_static/image11.png "agregar-a-create-view.png")

    *Adición de la vista de creación*
3. Actualización de la **GenreId** y **ArtistId** campos que se usarán una lista desplegable como se muestra a continuación:

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample13.cshtml)]

<a id="Ex4Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Tarea 3: ejecutar la aplicación

En esta tarea, probará que la **StoreManager** **crear** ver página muestra un formulario vacío del álbum.

1. Presione **F5** para ejecutar la aplicación.
2. El proyecto se inicia en la página principal. Cambiar la dirección URL de **/StoreManager/crear**. Compruebe que se muestra un formulario vacío para rellenar las nuevas propiedades del álbum.

    ![Crear vista con un formulario vacío](aspnet-mvc-4-helpers-forms-and-validation/_static/image12.png "Create View con un formulario vacío")

    *Crear vista con un formulario vacío*

<a id="Ex4Task4"></a>

<a id="Task_4_-_Implementing_the_HTTP-POST_Create_Action_Method"></a>
#### <a name="task-4---implementing-the-http-post-create-action-method"></a>Tarea 4: implementar el HTTP-POST crear método de acción

En esta tarea, implementará la versión HTTP-POST del método de acción Create que se invocará cuando un usuario hace clic en el **guardar** botón. El método debe guardar el nuevo álbum en la base de datos.

1. Cierre el explorador si es necesario, para volver a la ventana de Visual Studio. Abra **StoreManagerController** clase. Para ello, expanda el **controladores** carpeta y haga doble clic en **StoreManagerController.cs**.
2. Reemplace **crear HTTP-POST** código del método de acción con lo siguiente:

    (Código de fragmento de código - *las aplicaciones auxiliares de ASP.NET MVC 4 y formularios y validación: acción Ex4 StoreManagerController HTTP - POST crear*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample14.cs)]

    > [!NOTE]
    > La acción de creación es muy similar al método de acción de edición anterior, pero en lugar de establecer el objeto como modificada, se agrega al contexto.

<a id="Ex4Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Tarea 5: ejecución de la aplicación

En esta tarea, probará que la **StoreManager crear** ver página le permite crear un álbum nuevo y, a continuación, se redirige a la vista de índice StoreManager.

1. Presione **F5** para ejecutar la aplicación.
2. El proyecto se inicia en la página principal. Cambiar la dirección URL de **/StoreManager/crear**. Rellenar todos los campos de formulario con datos de un álbum nuevo, como el de la siguiente ilustración:

    ![Crear un álbum](aspnet-mvc-4-helpers-forms-and-validation/_static/image13.png "creando un álbum")

    *Crear un álbum*
3. Compruebe que se le redirigirá a la vista de índice StoreManager que incluye el nuevo álbum que acaba de crear.

    ![Nuevo álbum creado](aspnet-mvc-4-helpers-forms-and-validation/_static/image14.png "nuevo álbum creado")

    *Nuevo álbum creado*

<a id="Exercise5"></a>

<a id="Exercise_5_Handling_Deletion"></a>
### <a name="exercise-5-handling-deletion"></a>Ejercicio 5: Eliminación de control

La capacidad de eliminar álbumes aún no está implementada. Esto es lo que este ejercicio será aproximadamente. Como antes, se implementará el escenario de eliminación utilizando dos métodos independientes dentro de la **StoreManagerController** clase:

1. Un método de acción mostrará un formulario de confirmación
2. Un segundo método de acción va a controlar el envío del formulario

<a id="Ex5Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Delete_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-delete-action-method"></a>Tarea 1: implementación del método de acción Delete de HTTP-GET

En esta tarea, implementará la versión de HTTP-GET del método de acción Delete para recuperar información del álbum.

1. Abra el **comenzar** solución ubicado en **origen/Ex5-HandlingDeletion/inicio/** carpeta. En caso contrario, es posible que siga usando la **final** solución obtenido completando el ejercicio anterior.

   1. Si abrió proporcionado **comenzar** solución, deberá descargar algunos paquetes de NuGet que faltan antes de continuar. Para ello, haga clic en el **proyecto** menú y seleccione **administrar paquetes de NuGet**.
   2. En el **administrar paquetes de NuGet** cuadro de diálogo, haga clic en **restaurar** para descargar los paquetes que falten.
   3. Por último, compile la solución haciendo **compilar** | **compilar solución**.

      > [!NOTE]
      > Una de las ventajas de usar NuGet es que no tienen que enviar todas las bibliotecas en el proyecto, lo que reduce el tamaño del proyecto. Con las herramientas avanzadas de NuGet, mediante la especificación de las versiones del paquete en el archivo Packages.config, podrá descargar todas las bibliotecas requeridas en la primera vez que ejecute el proyecto. Esto es por eso tendrá que ejecutar estos pasos después de abrir una solución existente de este laboratorio.
2. Abra **StoreManagerController** clase. Para ello, expanda el **controladores** carpeta y haga doble clic en **StoreManagerController.cs**.
3. La acción de controlador de eliminación es exactamente igual que la acción del controlador anterior Store detalles: consulta el **álbum** objeto desde la base de datos mediante el **id** proporcionado en la dirección URL y devuelve el adecuado **vista**. Para ello, reemplace el HTTP-GET **eliminar** código del método de acción con lo siguiente:

    (Código de fragmento de código - *las aplicaciones auxiliares de ASP.NET MVC 4 y formularios y validación - HTTP-GET eliminación de control de Ex5 Eliminar acción*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample15.cs)]
4. Haga clic en el **eliminar** método de acción y seleccione **agregar vista**. Se abrirá el cuadro de diálogo Agregar vista.
5. En el cuadro de diálogo Agregar vista, compruebe que el nombre de vista es **eliminar**. Seleccione el **crear una vista fuertemente tipada** opción y seleccione **álbum (MvcMusicStore.Models)** desde el **clase modelo** lista desplegable. Seleccione **eliminar** desde el **plantilla Scaffold** lista desplegable. Deje los demás campos con sus valores predeterminados y, a continuación, haga clic en **agregar**.

    ![Agregar una vista Delete](aspnet-mvc-4-helpers-forms-and-validation/_static/image15.png "agregar una vista Delete")

    *Agregar una vista Delete*
6. La plantilla de eliminación muestra todos los campos del modelo. Se mostrará solo del álbum. Para ello, reemplace el contenido de la vista con el código siguiente:

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample16.cshtml)]

<a id="Ex05Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a>Tarea 2: ejecutar la aplicación

En esta tarea, probará que la **StoreManager** **eliminar** ver página muestra un formulario de eliminación de confirmación.

1. Presione **F5** para ejecutar la aplicación.
2. El proyecto se inicia en la página principal. Cambiar la dirección URL de **/StoreManager**. Seleccione un álbum que desea eliminar haciendo clic en **eliminar** y compruebe que se cargue la nueva vista.

    ![Al eliminar un álbum](aspnet-mvc-4-helpers-forms-and-validation/_static/image16.png "al eliminar un álbum")

    *Al eliminar un álbum*

<a id="Ex05Task3"></a>

<a id="Task_3-_Implementing_the_HTTP-POST_Delete_Action_Method"></a>
#### <a name="task-3--implementing-the-http-post-delete-action-method"></a>Tarea 3: implementar el método de acción Delete de HTTP-POST

En esta tarea, implementará la versión HTTP-POST del método de acción de eliminación que se invocará cuando un usuario hace clic en el **eliminar** botón. El método debe eliminar el álbum en la base de datos.

1. Cierre el explorador si es necesario, para volver a la ventana de Visual Studio. Abra **StoreManagerController** clase. Para ello, expanda el **controladores** carpeta y haga doble clic en **StoreManagerController.cs**.
2. Reemplace **HTTP-POST Delete** código del método de acción con lo siguiente:

    (Código de fragmento de código - *las aplicaciones auxiliares de ASP.NET MVC 4 y formularios y validación - HTTP-POST eliminación de control de Ex5 Eliminar acción*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample17.cs)]

<a id="Ex5Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Tarea 4: ejecución de la aplicación

En esta tarea, probará que la **StoreManager Delete** ver página permite eliminar un álbum y, a continuación, se redirige a la vista de índice StoreManager.

1. Presione **F5** para ejecutar la aplicación.
2. El proyecto se inicia en la página principal. Cambiar la dirección URL de **/StoreManager**. Seleccione un álbum que desea eliminar haciendo clic en **eliminar.** Haga clic en confirmar la eliminación **eliminar** botón:

    ![Al eliminar un álbum](aspnet-mvc-4-helpers-forms-and-validation/_static/image17.png "al eliminar un álbum")

    *Al eliminar un álbum*
3. Compruebe que se eliminó el álbum puesto que no aparece en el **índice** página.

<a id="Exercise6"></a>

<a id="Exercise_6_Adding_Validation"></a>
### <a name="exercise-6-adding-validation"></a>Ejercicio 6: Agregar validación

Actualmente, los formularios de Create y Edit con que de contar no realizan ningún tipo de validación. Si el usuario deja un campo obligatorio en blanco o escriba las letras en el campo de precio, será el primer error, obtendrá de la base de datos.

Puede agregar validación a la aplicación mediante la adición de anotaciones de datos a la clase de modelo. Las anotaciones de datos permiten que describe las reglas que desea aplicadas a las propiedades del modelo y ASP.NET MVC se encargará de aplicar y mostrar el mensaje adecuado a los usuarios.

<a id="Ex06Task1"></a>

<a id="Task_1_-_Adding_Data_Annotations"></a>
#### <a name="task-1---adding-data-annotations"></a>Tarea 1: agregar anotaciones de datos

En esta tarea, agregará las anotaciones de datos para el modelo de álbum que hará que la página Create y Edit mostrar mensajes de validación cuando corresponda.

Para una clase de modelo simple, agregar una anotación de datos se controla simplemente agregando un **mediante** instrucción para **System.ComponentModel.DataAnnotation**, a continuación, colocar un **[obligatorio]** atributo en las propiedades adecuadas. En el ejemplo siguiente se podría hacer que el **nombre** un campo obligatorio en la vista de la propiedad.

[!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample18.cs)]

Esto es un poco más complejo en casos como esta aplicación donde se genera el Entity Data Model. Si agrega las anotaciones de datos directamente a las clases del modelo, se sobrescribirán si actualiza el modelo de la base de datos. En su lugar, puede hacer uso de clases parciales de los metadatos que existirá para almacenar las anotaciones y están asociadas con el modelo de clases con el **[MetadataType]** atributo.

1. Abra el **comenzar** solución ubicado en **origen/Ex6-AddingValidation/inicio/** carpeta. En caso contrario, es posible que siga usando la **final** solución obtenido completando el ejercicio anterior.

   1. Si abrió proporcionado **comenzar** solución, deberá descargar algunos paquetes de NuGet que faltan antes de continuar. Para ello, haga clic en el **proyecto** menú y seleccione **administrar paquetes de NuGet**.
   2. En el **administrar paquetes de NuGet** cuadro de diálogo, haga clic en **restaurar** para descargar los paquetes que falten.
   3. Por último, compile la solución haciendo **compilar** | **compilar solución**.

      > [!NOTE]
      > Una de las ventajas de usar NuGet es que no tienen que enviar todas las bibliotecas en el proyecto, lo que reduce el tamaño del proyecto. Con las herramientas avanzadas de NuGet, mediante la especificación de las versiones del paquete en el archivo Packages.config, podrá descargar todas las bibliotecas requeridas en la primera vez que ejecute el proyecto. Esto es por eso tendrá que ejecutar estos pasos después de abrir una solución existente de este laboratorio.
2. Abra el **Album.cs** desde el **modelos** carpeta.
3. Reemplace **Album.cs** de contenido con el código resaltado, por lo que se parezca a lo siguiente:

    > [!NOTE]
    > La línea **[DisplayFormat(ConvertEmptyStringToNull=false)]** indica que no se puede convertir las cadenas vacías del modelo en null cuando se actualiza el campo de datos en el origen de datos. Esta configuración evitará una excepción cuando Entity Framework asigna valores null para el modelo antes de anotación de datos valida los campos.

    (Código de fragmento de código - *las aplicaciones auxiliares de ASP.NET MVC 4 y formularios y validación - clase parcial de metadatos de álbum Ex6*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample19.cs)]

    > [!NOTE]
    > Esto **álbum** clase parcial tiene un **MetadataType** atributo que apunta a la **AlbumMetaData** clase para las anotaciones de datos. Estos son algunos de los atributos de anotación de datos que se usa para anotar el modelo de álbum:
    > 
    > - Requerido: indica que la propiedad es un campo obligatorio
    > - DisplayName: define el texto que se usará en los campos de formulario y los mensajes de validación
    > - DisplayFormat: especifica cómo se muestran y formato de los campos de datos.
    > - StringLength - define una longitud máxima para un campo de cadena
    > - Intervalo: da como resultado un valor máximo y mínimo de un campo numérico
    > - ScaffoldColumn - permite la ocultación de campos de formularios de editor

<a id="Ex06Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a>Tarea 2: ejecutar la aplicación

En esta tarea, realizará las pruebas que las páginas Create y Edit validan campos, con los nombres para mostrar elegidos en la última tarea.

1. Presione **F5** para ejecutar la aplicación.
2. El proyecto se inicia en la página principal. Cambiar la dirección URL de **/StoreManager/crear**. Compruebe que los nombres coinciden con los de la clase parcial (como **URL carátulas de álbum** en lugar de **AlbumArtUrl**)
3. Haga clic en **crear**, sin rellenar el formulario. Compruebe que se obtienen los mensajes de validación correspondiente.

    ![Valida los campos en la página crear](aspnet-mvc-4-helpers-forms-and-validation/_static/image18.png "valida los campos en la página Crear")

    *Campos validados en la página Crear*
4. Puede comprobar que lo mismo ocurre con el **editar** página. Cambiar la dirección URL de **/StoreManager/Edit/1** y compruebe que los nombres coinciden con los de la clase parcial (como **URL carátulas de álbum** en lugar de **AlbumArtUrl**). Vacía la **título** y **precio** campos y haga clic en **guardar**. Compruebe que se obtienen los mensajes de validación correspondiente.

    ![Campos validados en la página de edición](aspnet-mvc-4-helpers-forms-and-validation/_static/image19.png)

    *Campos validados en la página de edición*

<a id="Exercise7"></a>

<a id="Exercise_7_Using_Unobtrusive_jQuery_at_Client_Side"></a>
### <a name="exercise-7-using-unobtrusive-jquery-at-client-side"></a>Ejercicio 7: Uso de jQuery discreta en el lado cliente

En este ejercicio, obtendrá información sobre cómo habilitar la validación de jQuery discreta de MVC 4 en el lado del cliente.

> [!NOTE]
> JQuery discreta usa el prefijo de datos ajax JavaScript para invocar métodos de acción en el servidor en lugar de manera intrusiva emitir los scripts de cliente en línea.


<a id="Ex7Task1"></a>

<a id="Task_1_-_Running_the_Application_before_Enabling_Unobtrusive_jQuery"></a>
#### <a name="task-1---running-the-application-before-enabling-unobtrusive-jquery"></a>Tarea 1: ejecutar la aplicación antes de habilitar discreta de jQuery

En esta tarea, ejecutará la aplicación antes de incluir jQuery con el fin de comparar ambos modelos de validación.

1. Abra el **comenzar** solución ubicado en **origen/Ex7-UnobtrusivejQueryValidation/inicio/** carpeta. En caso contrario, es posible que siga usando la **final** solución obtenido completando el ejercicio anterior.

   1. Si abrió proporcionado **comenzar** solución, deberá descargar algunos paquetes de NuGet que faltan antes de continuar. Para ello, haga clic en el **proyecto** menú y seleccione **administrar paquetes de NuGet**.
   2. En el **administrar paquetes de NuGet** cuadro de diálogo, haga clic en **restaurar** para descargar los paquetes que falten.
   3. Por último, compile la solución haciendo **compilar** | **compilar solución**.

      > [!NOTE]
      > Una de las ventajas de usar NuGet es que no tienen que enviar todas las bibliotecas en el proyecto, lo que reduce el tamaño del proyecto. Con las herramientas avanzadas de NuGet, mediante la especificación de las versiones del paquete en el archivo Packages.config, podrá descargar todas las bibliotecas requeridas en la primera vez que ejecute el proyecto. Esto es por eso tendrá que ejecutar estos pasos después de abrir una solución existente de este laboratorio.
2. Presione **F5** para ejecutar la aplicación.
3. El proyecto se inicia en la página principal. Examinar **/StoreManager/Create** y haga clic en **crear** sin rellenar el formulario para que compruebe que se obtienen mensajes de validación:

    ![Validación de cliente deshabilitado](aspnet-mvc-4-helpers-forms-and-validation/_static/image20.png "validación de cliente deshabilitado")

    *Validación de cliente deshabilitado*
4. En el explorador, abra el código fuente HTML:

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample20.html)]

<a id="Ex7Task2"></a>

<a id="Task_2_-_Enabling_Unobtrusive_Client_Validation"></a>
#### <a name="task-2---enabling-unobtrusive-client-validation"></a>Tarea 2: Habilitar validación del cliente discreta

En esta tarea, permitirá jQuery **validación del cliente discreta** desde **Web.config** archivo, que se establece en false en todos los nuevos proyectos de ASP.NET MVC 4 de forma predeterminada. Además, agregará que las referencias para realizar el trabajo de validación de cliente discreta de jQuery de secuencias de comandos necesarios.

1. Abra **Web.Config** de archivos en la raíz del proyecto y asegúrese de que el **ClientValidationEnabled** y **UnobtrusiveJavaScriptEnabled** valores de las claves se establecen en **true**.

    [!code-xml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample21.xml)]

    > [!NOTE]
    > También puede habilitar la validación del cliente mediante código a Global.asax.cs para obtener los mismos resultados:
    > 
    > **HtmlHelper.ClientValidationEnabled = true;**
    > 
    > Además, puede asignar el atributo de ClientValidationEnabled en cualquier controlador de tener un comportamiento personalizado.
2. Abra **Create.cshtml** en **Views\StoreManager**.
3. Asegúrese de que los siguientes archivos de script, **jquery.validate** y **jquery.validate.unobtrusive**, se hace referencia en el a través de la vista la &quot; **~/bundles/jqueryval** &quot; agrupación.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample22.cshtml)]

    > [!NOTE]
    > Todas estas bibliotecas de jQuery se incluyen en los nuevos proyectos de MVC 4. Puede encontrar más bibliotecas en el **/Scripts** carpeta del proyecto.
    > 
    > Para realizar esta validación bibliotecas funcionan, deberá agregar una referencia a la biblioteca del marco de jQuery. Puesto que esta referencia se ha agregado en el  **\_Layout.cshtml** archivo, no es necesario agregarlo en esta vista.

<a id="Ex7Task3"></a>

<a id="Task_3_-_Running_the_Application_Using_Unobtrusive_jQuery_Validation"></a>
#### <a name="task-3---running-the-application-using-unobtrusive-jquery-validation"></a>Tarea 3: ejecutar la validación de jQuery discreta de uso de aplicación

En esta tarea, probará que la **StoreManager** Crear vista plantilla realiza la validación del lado cliente mediante bibliotecas de jQuery cuando el usuario crea un nuevo álbum.

1. Presione **F5** para ejecutar la aplicación.
2. El proyecto se inicia en la página principal. Examinar **/StoreManager/Create** y haga clic en **crear** sin rellenar el formulario para que compruebe que se obtienen mensajes de validación:

    ![Validación del cliente con jQuery habilitado](aspnet-mvc-4-helpers-forms-and-validation/_static/image21.png "validación del cliente con jQuery habilitado")

    *Validación del cliente con jQuery habilitado*
3. En el explorador, abra el código fuente de la vista de creación:

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample23.html)]

   > [!NOTE]
   > Para cada regla de validación del cliente, jQuery discreta agrega un atributo con datos-val -*rulename*=&quot;*mensaje*&quot;. A continuación se presenta una lista de etiquetas que discreta jQuery se inserta en el campo de entrada html para realizar la validación de cliente:
   > 
   > - Datos val
   > - Número de datos val
   > - Rango de datos val
   > - Datos-val-range-min / datos-val-range-Max.
   > - Datos necesarios para val
   > - Longitud de datos val
   > - Datos val longitud máxima / datos val longitud mínima
   > 
   > Todos los valores de datos se rellenan con el modelo **anotación de datos**. A continuación, se puede ejecutar toda la lógica que funciona en el lado del servidor en el lado del cliente. Por ejemplo, el atributo precio tiene la siguiente anotación de datos en el modelo:
   > 
   > [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample24.cs)]
   > 
   > Después de usar jQuery discreta, el código generado es:
   > 
   > [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample25.html)]

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Resumen

Al completar este laboratorio práctico ha aprendido cómo habilitar usuarios cambiar los datos almacenados en la base de datos con el uso de las siguientes acciones:

- Las acciones de controlador, como Index, Create, Edit, Delete
- Característica de scaffolding de ASP.NET MVC para mostrar las propiedades en una tabla HTML
- Experimentan de aplicaciones auxiliares HTML personalizadas para mejorar el usuario
- Métodos de acción que reaccionan a HTTP-GET o las llamadas HTTP-POST
- Una plantilla de editor compartida para las plantillas de vista similares, como Create y Edit
- Elementos de formulario como listas desplegables
- Anotaciones de datos para la validación del modelo
- Validación del lado cliente mediante la biblioteca de jQuery discreta

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Apéndice A: instalación de Visual Studio Express 2012 para Web

Puede instalar **Microsoft Visual Studio Express 2012 para Web** u otro &quot;Express&quot; versión utilizando el **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**. Las instrucciones siguientes le guían a través de los pasos necesarios para instalar *Visual studio Express 2012 para Web* mediante *Microsoft Web Platform Installer*.

1. Vaya a [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169). Como alternativa, si ya ha instalado el instalador de plataforma Web, puede abrirla y busque el producto &quot; <em>Visual Studio Express 2012 for Web con SDK de Windows Azure</em>&quot;.
2. Haga clic en **instalar ahora**. Si no tienes **instalador de plataforma Web** se le redirigirá para descargarlo e instalarlo en primer lugar.
3. Una vez **instalador de plataforma Web** está abierto, haga clic en **instalar** para iniciar el programa de instalación.

    ![Instale Visual Studio Express](aspnet-mvc-4-helpers-forms-and-validation/_static/image22.png "instale Visual Studio Express")

    *Instale Visual Studio Express*
4. Leer todos los productos las licencias y los términos y haga clic en **acepto** para continuar.

    ![Acepte los términos de licencia](aspnet-mvc-4-helpers-forms-and-validation/_static/image23.png)

    *Acepte los términos de licencia*
5. Espere hasta que finalice el proceso de descarga e instalación.

    ![Progreso de la instalación](aspnet-mvc-4-helpers-forms-and-validation/_static/image24.png)

    *Progreso de la instalación*
6. Cuando se complete la instalación, haga clic en **finalizar**.

    ![Instalación completada](aspnet-mvc-4-helpers-forms-and-validation/_static/image25.png)

    *Instalación completada*
7. Haga clic en **Exit** para cerrar el instalador de plataforma Web.
8. Para abrir Visual Studio Express para Web, vaya a la **iniciar** pantalla y comienza a escribir &quot; **VS Express**&quot;, a continuación, haga clic en el **VS Express para Web** icono.

    ![VS Express para el icono de Web](aspnet-mvc-4-helpers-forms-and-validation/_static/image26.png)

    *VS Express para el icono de Web*

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a>Apéndice B: uso de fragmentos de código

Con fragmentos de código, tiene todo el código que necesita a su alcance. El documento de laboratorio le dirá exactamente cuándo se pueden utilizar, como se muestra en la ilustración siguiente.

![Uso de fragmentos de código de Visual Studio para insertar código en el proyecto](aspnet-mvc-4-helpers-forms-and-validation/_static/image27.png "fragmentos de código en Visual Studio para insertar código en el proyecto")

*Uso de fragmentos de código de Visual Studio para insertar código en el proyecto*

***Para agregar un fragmento de código mediante el teclado (solo C#)***

1. Coloque el cursor donde desea insertar el código.
2. Comience a escribir el nombre del fragmento de código (sin espacios ni guiones).
3. Observe cómo IntelliSense muestra los nombres de fragmentos de código coincidentes.
4. Seleccione el fragmento de código correcto (o siga escribiendo hasta que se selecciona el nombre del fragmento de código completo).
5. Presione la tecla Tab dos veces para insertar el fragmento de código en la ubicación del cursor.

![Comience a escribir el nombre del fragmento](aspnet-mvc-4-helpers-forms-and-validation/_static/image28.png "comience a escribir el nombre del fragmento de código")

*Comience a escribir el nombre del fragmento de código*

![Presione la tecla Tab para seleccionar el fragmento de código resaltada](aspnet-mvc-4-helpers-forms-and-validation/_static/image29.png "presione Tab para seleccionar el fragmento de código resaltada")

*Presione la tecla Tab para seleccionar el fragmento de código resaltada*

![Vuelva a presionar Tab y el fragmento de código se expandirán](aspnet-mvc-4-helpers-forms-and-validation/_static/image30.png "vuelva a presionar Tab y el fragmento de código se expandirán")

*Vuelva a presionar Tab y el fragmento de código se expandirán*

***Para agregar un fragmento de código con el mouse (C#, en Visual Basic y XML)*** 1. Haga doble clic en el desea insertar el fragmento de código.

1. Seleccione **Insertar fragmento de código** seguido **Mis fragmentos de código**.
2. Seleccione el fragmento de código relevante en la lista, haciendo clic en él.

![Con el botón secundario donde desea insertar el fragmento de código y seleccione Insertar fragmento de código](aspnet-mvc-4-helpers-forms-and-validation/_static/image31.png "contextual donde desea insertar el fragmento de código y seleccione Insertar fragmento de código")

*Haga doble clic donde desea insertar el fragmento de código y seleccione Insertar fragmento de código*

![Elegir el fragmento de código relevante en la lista, hacer clic en ella](aspnet-mvc-4-helpers-forms-and-validation/_static/image32.png "elegir el fragmento de código relevante en la lista, hacer clic en ella")

*Elegir el fragmento de código relevante en la lista, hacer clic en ella*
