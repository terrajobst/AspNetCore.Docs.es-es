---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-custom-action-filters
title: Los filtros de acción personalizada de ASP.NET MVC 4 | Documentos de Microsoft
author: rick-anderson
description: ASP.NET MVC proporciona filtros de acción para ejecutar lógica de filtrado antes o después de llama a un método de acción. Los filtros de acción son atributos personalizados tha...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 969ab824-1b98-4552-81fe-b60ef5fc6887
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-custom-action-filters
msc.type: authoredcontent
ms.openlocfilehash: 8b135b23aea64b0c7c7d4368eef9ee80914159e4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
# <a name="aspnet-mvc-4-custom-action-filters"></a>Filtros de acción personalizada de ASP.NET MVC 4

Por [Web colonias equipo](https://twitter.com/webcamps)

[Descargar el Kit de aprendizaje de colonias de Web](https://aka.ms/webcamps-training-kit)

ASP.NET MVC proporciona filtros de acción para ejecutar lógica de filtrado antes o después de llama a un método de acción. Filtros de acción son atributos personalizados que proporcionan un medio declarativo para agregar comportamiento previo y posterior a la acción a los métodos de acción del controlador.

En este laboratorio práctico creará un atributo de filtro de acción personalizada a la solución de MvcMusicStore para detectar las solicitudes del controlador y registra la actividad de un sitio en una tabla de base de datos. Podrá agregar el filtro de registro por inyección de código a cualquier acción o controlador. Por último, verá la vista del registro que muestra la lista de los visitantes.

Este laboratorio práctico se da por supuesto que tiene conocimientos básicos de **ASP.NET MVC**. Si no ha usado **ASP.NET MVC** antes, le recomendamos que repase **Fundamentos de ASP.NET MVC 4** laboratorio práctico.

> [!NOTE]
> Todo el código de ejemplo y fragmentos de código se incluyen en el Kit de aprendizaje de Web colonias, disponible desde en [versiones de Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit). Está disponible en el proyecto específico para este laboratorio [filtros de acción personalizados de ASP.NET MVC 4](https://github.com/Microsoft-Web/HOL-MVC4CustomActionFilters).

<a id="Objectives"></a>
### <a name="objectives"></a>Objetivos

En este laboratorio práctico, aprenderá cómo:

- Cree un atributo de filtro de acción personalizada para ampliar las capacidades de filtrado
- Aplicar un atributo de filtro personalizado mediante la inserción de un nivel específico
- Registrar los filtros de acción personalizada global

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

Si no está familiarizado con los fragmentos de código de Visual Studio y desea obtener información sobre cómo utilizarlas, puede consultar el apéndice de este documento &quot; [Apéndice C: Using Code Snippets](#AppendixC)&quot;.

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Ejercicios

Este laboratorio práctico se compone de los ejercicios siguientes:

1. [Ejercicio 1: Registrar acciones](#Exercise1)
2. [Ejercicio 2: Administración de varios filtros de acción](#Exercise2)

Tiempo estimado para completar esta práctica: **30 minutos**.

> [!NOTE]
> Cada ejercicio está acompañado por un **final** carpeta que contiene la solución resultante debería obtener después de completar los ejercicios. Puede usar esta solución como una guía si necesita ayuda adicional para trabajar a través de los ejercicios.


<a id="Exercise1"></a>

<a id="Exercise_1_Logging_Actions"></a>
### <a name="exercise-1-logging-actions"></a>Ejercicio 1: Registrar acciones

En este ejercicio, obtendrá información sobre cómo crear un filtro de registro de acción personalizada mediante el uso de proveedores de filtros de ASP.NET MVC 4. Para ello aplicará un filtro de registro para el sitio de MusicStore que registra todas las actividades en los controladores seleccionados.

El filtro se ampliará **ActionFilterAttributeClass** e invalide **OnActionExecuting** método para detectar cada solicitud y, a continuación, realice las acciones de registro. Ejecutar métodos, resultados y parámetros se proporcionará información de contexto sobre solicitudes HTTP, ASP.NET MVC **ResultExecutingContext** clase **.**

> [!NOTE]
> ASP.NET MVC 4 también tiene proveedores de filtros predeterminados que puede utilizar sin crear un filtro personalizado. ASP.NET MVC 4 proporciona los siguientes tipos de filtros:
> 
> - **Autorización** filtrar, que toma decisiones de seguridad sobre si se debe ejecutar un método de acción, como realizar la autenticación o validar las propiedades de la solicitud.
> - **Acción** filtro, que encapsula la ejecución del método de acción. Este filtro puede llevar a cabo un procesamiento adicional, como proporcionar datos extra al método de acción, inspeccionar el valor devuelto o cancelar la ejecución del método de acción
> - **Resultado** filtro, que encapsula la ejecución del objeto ActionResult. Este filtro puede llevar a cabo un procesamiento adicional del resultado, como modificar la respuesta HTTP.
> - **Excepción** filtro, que se ejecuta si hay una excepción no controlada producida en algún lugar en el método de acción, empezando por los filtros de autorización y finalizando con la ejecución del resultado. Filtros de excepciones se pueden utilizar para tareas como registrar o mostrar una página de error.
> 
> Para obtener más información acerca de los proveedores de filtros, visite este vínculo MSDN: ([https://msdn.microsoft.com/library/dd410209.aspx](https://msdn.microsoft.com/library/dd410209.aspx)).


<a id="AboutLoggingFeature"></a>

<a id="About_MVC_Music_Store_Application_logging_feature"></a>
#### <a name="about-mvc-music-store-application-logging-feature"></a>Acerca de la característica de registro de aplicación de tienda de música de MVC

Esta solución de la tienda de música tiene una nueva tabla de modelo de datos para el registro de sitio, **ActionLog**, con los siguientes campos: nombre del controlador que recibió una solicitud, acción de llamadas, dirección IP del cliente y marca de tiempo.

![Modelo de datos. Tabla ActionLog. ](aspnet-mvc-4-custom-action-filters/_static/image1.png "Modelo de datos. Tabla ActionLog.")

*Modelos de datos, tabla de ActionLog*

La solución proporciona una vista de MVC de ASP.NET para el registro de acciones que se pueden encontrar en **MvcMusicStores/vistas/ActionLog**:

![Vista de registro de acción](aspnet-mvc-4-custom-action-filters/_static/image2.png "vista de registro de acciones")

*Vista de registro de acción*

Con esto tiene la estructura, todo el trabajo se centrará en interrumpir la solicitud del controlador y realizar el registro mediante el uso de filtrado personalizado.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_a_Custom_Filter_to_Catch_a_Controllers_Request"></a>
#### <a name="task-1---creating-a-custom-filter-to-catch-a-controllers-request"></a>Tarea 1: crear un filtro personalizado para detectar la solicitud del controlador

En esta tarea creará una clase de atributo de filtro personalizado que contendrá la lógica de registro. Para ello se ampliará ASP.NET MVC **ActionFilterAttribute** clase e implemente la interfaz **IActionFilter**.

> [!NOTE]
> El **ActionFilterAttribute** es la clase base para todos los filtros de atributo. Proporciona los métodos siguientes para ejecutar una lógica específica después y antes de la ejecución de la acción de controlador:
> 
> - **OnActionExecuting**(ResultExecutingContext filterContext): justo antes de la acción se denomina método.
> - **OnActionExecuted**(ActionExecutedContext filterContext): una vez que se llama al método de acción y antes de que se ejecuta el resultado (antes de la representación de vista).
> - **OnResultExecuting**(ActionExecutingContext filterContext): justo antes de que se ejecuta el resultado (antes de la representación de vista).
> - **OnResultExecuted**(ResultExecutedContext filterContext): después de ejecuta el resultado (después de que se represente la vista).
> 
> Al reemplazar cualquiera de estos métodos en una clase derivada, es posible ejecutar su propio código de filtrado.


1. Abra la **comenzar** solución ubicado en **\Source\Ex01-LoggingActions\Begin** carpeta.

   1. Debe descargar algunos paquetes de NuGet que faltan antes de continuar. Para ello, haga clic en el **proyecto** menú y seleccione **administrar paquetes de NuGet**.
   2. En el **administrar paquetes de NuGet** cuadro de diálogo, haga clic en **restaurar** para descargar los paquetes que falten.
   3. Por último, compile la solución haciendo clic en **generar** | **generar solución**.

      > [!NOTE]
      > Una de las ventajas del uso de NuGet es que no tiene que enviar todas las bibliotecas en el proyecto, lo que reduce el tamaño del proyecto. Con NuGet Power Tools, mediante la especificación de las versiones del paquete en el archivo Packages.config, podrá descargar la primera vez que ejecute el proyecto de todas las bibliotecas necesarias. Este es el motivo por el que se deben ejecutar estos pasos después de abrir una solución existente de este laboratorio.
      > 
      > Para obtener más información, consulte este artículo: [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages ](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).
2. Agregue una nueva clase de C# en el **filtros** carpeta y asígnele el nombre *CustomActionFilter.cs*. Esta carpeta almacenará todos los filtros personalizados.
3. Abra **CustomActionFilter.cs** y agregue una referencia a **System.Web.Mvc** y **MvcMusicStore.Models** espacios de nombres:

    (Código de fragmento de código: *filtros de acción personalizada de ASP.NET MVC 4 - Ex1 CustomActionFilterNamespaces*)

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample1.cs)]
4. Heredar la **CustomActionFilter** clase **ActionFilterAttribute** y, a continuación, realice **CustomActionFilter** clase implemente **IActionFilter** interfaz.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample2.cs)]
5. Asegúrese de **CustomActionFilter** clase invalide el método **OnActionExecuting** y agregue la lógica necesaria para registrar la ejecución del filtro. Para ello, agregue el código que aparece resaltado en **CustomActionFilter** clase.

    (Código de fragmento de código: *filtros de acción personalizada de ASP.NET MVC 4 - Ex1 LoggingActions*)

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample3.cs#Highlight)]

    > [!NOTE]
    > **OnActionExecuting** método consiste en utilizar **Entity Framework** para agregar un nuevo registro de ActionLog. Crea y rellena una nueva instancia de entidad con la información de contexto de **filterContext**.
    > 
    > Más información sobre **elemento ControllerContext** clase en [msdn](https://msdn.microsoft.com/library/system.web.mvc.controllercontext.aspx).

<a id="Ex1Task2"></a>

<a id="Task_2_-_Injecting_a_Code_Interceptor_into_the_Store_Controller_Class"></a>
#### <a name="task-2---injecting-a-code-interceptor-into-the-store-controller-class"></a>Tarea 2: insertar un Interceptor de código en la clase de controlador de almacén

En esta tarea, agregará el filtro personalizado insertando a todas las clases de controlador y las acciones de controlador que se registrarán. En este ejercicio, la clase de controlador de almacén tendrá un registro.

El método **OnActionExecuting** de **ActionLogFilterAttribute** filtro personalizado se ejecuta cuando se llama a un elemento insertado.

También es posible interceptar un método de un controlador específico.

1. Abra la **StoreController** en **MvcMusicStore\Controllers** y agregue una referencia a la **filtros** espacio de nombres:

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample4.cs)]
2. Insertar el filtro personalizado **CustomActionFilter** en **StoreController** clase agregando **[CustomActionFilter]** atributo antes de la declaración de clase.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample5.cs)]

   > [!NOTE]
   > Cuando se aplica un filtro en una clase de controlador, también se insertan todas sus acciones. Si desea que se aplica el filtro sólo para un conjunto de acciones, tendría que insertar **[CustomActionFilter]** a cada uno de ellos:
   > 
   > [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample6.cs)]

<a id="Ex1Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Tarea 3: ejecutar la aplicación

En esta tarea, probará que funciona el filtro de registro. Se iniciará la aplicación y visitar la tienda y, a continuación, comprobará las actividades ha iniciado.

1. Presione **F5** para ejecutar la aplicación.
2. Vaya a **/ActionLog** para ver el estado inicial de la vista de registro:

    ![Estado de seguimiento antes de la actividad de las páginas del registro](aspnet-mvc-4-custom-action-filters/_static/image3.png "estado de seguimiento antes de la actividad de las páginas del registro")

    *Estado de seguimiento de registro antes de la actividad de la página*

   > [!NOTE]
   > De forma predeterminada, siempre mostrará un elemento que se genera cuando se recuperan los géneros existentes para el menú.
   > 
   > Para fines de simplificar nos estamos limpiar el **ActionLog** tabla cada vez que se ejecuta la aplicación, por lo que sólo mostrará los registros de comprobación de cada tarea determinada.
   > 
   > Tendrá que quitar el código siguiente desde el **sesión\_iniciar** (método) (en el **Global.asax** clase), con el fin de guardar un registro histórico de todas las acciones que se ejecuta en el almacén Controlador.
   > 
   > [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample7.cs)]
3. Haga clic en uno de los **géneros** en el menú y realizar algunas acciones, como navegar por un álbum disponible.
4. Vaya a **/ActionLog** y si el registro está vacío presione **F5** para actualizar la página. Compruebe que se realizó el seguimiento de las visitas:

    ![Registro de acciones con el registro de actividad](aspnet-mvc-4-custom-action-filters/_static/image4.png "registro de acciones con el registro de actividad")

    *Registro de acciones con el registro de actividad*

<a id="Exercise2"></a>

<a id="Exercise_2_Managing_Multiple_Action_Filters"></a>
### <a name="exercise-2-managing-multiple-action-filters"></a>Ejercicio 2: Administración de varios filtros de acción

En este ejercicio agregará un segundo filtro de acción personalizado a la clase StoreController y definir el orden específico en el que se ejecutarán ambos filtros. A continuación, actualizará el código para registrar el filtro global.

Hay diferentes opciones para tener en cuenta al definir el orden de ejecución de los filtros. Por ejemplo, la propiedad orden y ámbito de los filtros:

Puede definir un **ámbito** para cada uno de los filtros, por ejemplo, puede definir el ámbito todos los filtros de acción para que se ejecute dentro de la **controlador ámbito**y todos los filtros de autorización para que se ejecute **ámbito Global** . Los ámbitos tienen un orden de ejecución definido.

Además, cada filtro de acción tiene una propiedad de orden que se usa para determinar el orden de ejecución en el ámbito del filtro.

Para obtener más información sobre el orden de ejecución de filtros de acción personalizados, visite este artículo MSDN: ([https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx](https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx)).

<a id="Ex2Task1"></a>

<a id="Task_1_Creating_a_new_Custom_Action_Filter"></a>
#### <a name="task-1-creating-a-new-custom-action-filter"></a>Tarea 1: Crear un nuevo filtro de acción personalizado

En esta tarea, creará un nuevo filtro de acción personalizado para insertar en la clase StoreController, obtener información sobre cómo administrar el orden de ejecución de los filtros.

1. Abra la **comenzar** solución ubicado en **\Source\Ex02-ManagingMultipleActionFilters\Begin** carpeta. En caso contrario, puede seguir usando el **final** solución obtenido siguiendo el ejercicio anterior.

    1. Si abrió proporcionado **comenzar** solución, deberá descargar algunos paquetes de NuGet que faltan antes de continuar. Para ello, haga clic en el **proyecto** menú y seleccione **administrar paquetes de NuGet**.
    2. En el **administrar paquetes de NuGet** cuadro de diálogo, haga clic en **restaurar** para descargar los paquetes que falten.
    3. Por último, compile la solución haciendo clic en **generar** | **generar solución**.

        > [!NOTE]
        > Una de las ventajas del uso de NuGet es que no tiene que enviar todas las bibliotecas en el proyecto, lo que reduce el tamaño del proyecto. Con NuGet Power Tools, mediante la especificación de las versiones del paquete en el archivo Packages.config, podrá descargar la primera vez que ejecute el proyecto de todas las bibliotecas necesarias. Este es el motivo por el que se deben ejecutar estos pasos después de abrir una solución existente de este laboratorio.
        > 
        > Para obtener más información, consulte este artículo: [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages ](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).
2. Agregue una nueva clase de C# en el **filtros** carpeta y asígnele el nombre *MyNewCustomActionFilter.cs*
3. Abra **MyNewCustomActionFilter.cs** y agregue una referencia a **System.Web.Mvc** y **MvcMusicStore.Models** espacio de nombres:

    (Código de fragmento de código: *filtros de acción personalizada de ASP.NET MVC 4 - Ex2 MyNewCustomActionFilterNamespaces*)

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample8.cs)]
4. Reemplace la declaración de clase predeterminada por el código siguiente.

    (Código de fragmento de código: *filtros de acción personalizada de ASP.NET MVC 4 - Ex2 MyNewCustomActionFilterClass*)

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample9.cs)]

    > [!NOTE]
    > Este filtro de acción personalizado es casi idéntico a la que creó en el ejercicio anterior. La principal diferencia es que tiene el *&quot;iniciado por&quot;* atributo actualizado con el nombre de esta nueva clase para identificar el filtro wich registra el registro.

<a id="Ex2Task2"></a>

<a id="Task_2_Injecting_a_new_Code_Interceptor_into_the_StoreController_Class"></a>
#### <a name="task-2-injecting-a-new-code-interceptor-into-the-storecontroller-class"></a>Tarea 2: Insertar un Interceptor de código nuevo en la clase StoreController

En esta tarea, agregará un nuevo filtro personalizado en la clase StoreController y ejecutar la solución para comprobar cómo ambos filtros funcionan juntos.

1. Abra la **StoreController** clase ubicado en **MvcMusicStore\Controllers** e inserta el nuevo filtro personalizado **MyNewCustomActionFilter** en  **StoreController** clase como se muestra en el código siguiente.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample10.cs)]
2. Ahora, ejecute la aplicación para ver cómo funcionan estos dos filtros de acción personalizada. Para ello, presione **F5** y espere hasta que se inicie la aplicación.
3. Vaya a **/ActionLog** para ver el estado inicial de la vista de registro.

    ![Estado de seguimiento antes de la actividad de las páginas del registro](aspnet-mvc-4-custom-action-filters/_static/image5.png "estado de seguimiento antes de la actividad de las páginas del registro")

    *Estado de seguimiento de registro antes de la actividad de la página*
4. Haga clic en uno de los **géneros** en el menú y realizar algunas acciones, como navegar por un álbum disponible.
5. Compruebe que en este momento; las visitas se realizó el seguimiento dos veces: una vez para cada uno de los filtros de acción personalizado que agregó en el **controladora de almacenamiento** clase.

    ![Registro de acciones con el registro de actividad](aspnet-mvc-4-custom-action-filters/_static/image6.png "registro de acciones con el registro de actividad")

    *Registro de acciones con el registro de actividad*
6. Cierre el explorador.

<a id="Ex2Task3"></a>

<a id="Task_3_Managing_Filter_Ordering"></a>
#### <a name="task-3-managing-filter-ordering"></a>Tarea 3: Administrar el orden de los filtros

En esta tarea, aprenderá a administrar el orden de ejecución de los filtros mediante la propiedad de orden.

1. Abra la **StoreController** clase ubicado en **MvcMusicStore\Controllers** y especifique la **orden** propiedad en ambos filtros como se muestra a continuación.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample11.cs)]
2. Ahora, compruebe cómo se ejecutan los filtros según el valor de la propiedad de su orden. Encontrará que el filtro con el valor de orden más pequeño (**CustomActionFilter**) es el primero que se ejecuta. Presione **F5** y espere hasta que se inicie la aplicación.
3. Vaya a **/ActionLog** para ver el estado inicial de la vista de registro.

    ![Estado de seguimiento antes de la actividad de las páginas del registro](aspnet-mvc-4-custom-action-filters/_static/image7.png "estado de seguimiento antes de la actividad de las páginas del registro")

    *Estado de seguimiento de registro antes de la actividad de la página*
4. Haga clic en uno de los **géneros** en el menú y realizar algunas acciones, como navegar por un álbum disponible.
5. Compruebe que este momento, las visitas se realizó el seguimiento se ordenan por valor de orden de los filtros: **CustomActionFilter** registros primero.

    ![Registro de acciones con el registro de actividad](aspnet-mvc-4-custom-action-filters/_static/image8.png "registro de acciones con el registro de actividad")

    *Registro de acciones con el registro de actividad*
6. Ahora, debe actualizar el valor de orden de los filtros y comprobar cómo se cambia el orden de registro. En el **StoreController** clase, actualice el valor de orden de los filtros como se muestra a continuación.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample12.cs)]
7. Vuelva a ejecutar la aplicación presionando **F5**.
8. Haga clic en uno de los **géneros** en el menú y realizar algunas acciones, como navegar por un álbum disponible.
9. Compruebe que este momento, los registros crean por **MyNewCustomActionFilter** filtro aparece en primer lugar.

    ![Registro de acciones con el registro de actividad](aspnet-mvc-4-custom-action-filters/_static/image9.png "registro de acciones con el registro de actividad")

    *Registro de acciones con el registro de actividad*

<a id="Ex2Task4"></a>

<a id="Task_4_Registering_Filters_Globally"></a>
#### <a name="task-4-registering-filters-globally"></a>Tarea 4: Registrar filtros global

En esta tarea, actualizará la solución para registrar el nuevo filtro (**MyNewCustomActionFilter**) como un filtro global. Al hacerlo, se activará por la realiza acciones en la aplicación y no solo en el que StoreController como se muestra en la tarea anterior.

1. En **StoreController** clase, quite **[MyNewCustomActionFilter]** atributo y la propiedad order de **[CustomActionFilter]**. Debería ser similar al siguiente:

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample13.cs)]
2. Abra **Global.asax** de archivos y busque la **aplicación\_iniciar** método. Observe que cada vez que inicia la aplicación está registrando los filtros globales mediante una llamada a **RegisterGlobalFilters** método dentro de **FilterConfig** clase.

    ![Registro de filtros globales en Global.asax](aspnet-mvc-4-custom-action-filters/_static/image10.png "registrar filtros globales en Global.asax")

    *Registro de filtros globales en Global.asax*
3. Abra **FilterConfig.cs** archivo dentro de **aplicación\_iniciar** carpeta.
4. Agregue una referencia al uso de System.Web.Mvc; uso de MvcMusicStore.Filters; espacio de nombres.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample14.cs)]
5. Actualización **RegisterGlobalFilters** método Agregar filtro personalizado. Para ello, agregue el código resaltado:

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample15.cs)]
6. Ejecute la aplicación presionando **F5**.
7. Haga clic en uno de los **géneros** en el menú y realizar algunas acciones, como navegar por un álbum disponible.
8. Compruebe que ahora **[MyNewCustomActionFilter]** es que se va a insertar en HomeController y ActionLogController demasiado.

    ![Registro de acciones con el registro de actividad](aspnet-mvc-4-custom-action-filters/_static/image11.png "registro de acciones con el registro de actividad")

    *Registro de acciones con registro de actividad global*

> [!NOTE]
> Además, puede implementar esta aplicación a los siguientes sitios Web Windows Azure [Apéndice B: publicar una aplicación de ASP.NET MVC 4 mediante Web Deploy](#AppendixB).


* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Resumen

Al completar este laboratorio práctico ha aprendido cómo ampliar un filtro de acción para ejecutar acciones personalizadas. También ha aprendido cómo insertar cualquier filtro para los controladores de página. Se usaron los siguientes conceptos:

- Cómo crear filtros de acción personalizado con la clase ActionFilterAttribute de MVC de ASP.NET
- Cómo insertar filtros en los controladores de ASP.NET MVC
- Cómo administrar el orden de los filtros utilizando la propiedad Order
- Cómo registrar filtros global

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Apéndice A: instalación de Visual Studio Express 2012 para Web

Puede instalar **Microsoft Visual Studio Express 2012 para Web** u otro &quot;Express&quot; versión usando la **[instalador de plataforma Web de Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)**. Las instrucciones siguientes le guían a través de los pasos necesarios para instalar *Visual studio Express 2012 para Web* con *instalador de plataforma Web de Microsoft*.

1. Vaya a [ [ https://go.microsoft.com/? linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). O bien, si ya ha instalado el instalador de plataforma Web, puede abrirla y busque el producto &quot; <em>Visual Studio Express 2012 for Web con SDK de Windows Azure</em>&quot;.
2. Haga clic en **instalar ahora**. Si no tiene **instalador de plataforma Web** se le redirigirá para descargarlo e instalarlo primero.
3. Una vez **instalador de plataforma Web** está abierto, haga clic en **instalar** para iniciar el programa de instalación.

    ![Instale Visual Studio Express](aspnet-mvc-4-custom-action-filters/_static/image12.png "instale Visual Studio Express")

    *Instale Visual Studio Express*
4. Lea los términos y licencias de todos los productos y haga clic en **acepto** para continuar.

    ![Acepte los términos de licencia](aspnet-mvc-4-custom-action-filters/_static/image13.png)

    *Acepte los términos de licencia*
5. Espere hasta que finalice el proceso de descarga e instalación.

    ![Progreso de la instalación](aspnet-mvc-4-custom-action-filters/_static/image14.png)

    *Progreso de la instalación*
6. Cuando se complete la instalación, haga clic en **finalizar**.

    ![Se completó la instalación](aspnet-mvc-4-custom-action-filters/_static/image15.png)

    *Se completó la instalación*
7. Haga clic en **Exit** para cerrar el instalador de plataforma Web.
8. Para abrir Visual Studio Express para Web, vaya a la **iniciar** pantalla y comienza a escribir &quot; **Express frente a**&quot;, a continuación, haga clic en el **VS Express para Web** colocar en mosaico.

    ![Express de VS para icono Web](aspnet-mvc-4-custom-action-filters/_static/image16.png)

    *Express de VS para icono Web*

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Apéndice B: publicar una aplicación de ASP.NET MVC 4 mediante Web Deploy

Este apéndice le mostrará cómo crear un nuevo sitio web del Portal de administración de Windows Azure y publicar la aplicación que obtuvo siguiendo el laboratorio, aprovechando la característica de publicación Web Deploy proporcionada por Windows Azure.

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a>Tarea 1: crear un nuevo sitio Web desde las ventanas de Portal de Azure

1. Vaya a la [Portal de administración de Windows Azure](https://manage.windowsazure.com/) e inicie sesión con las credenciales de Microsoft asociadas con su suscripción.

    > [!NOTE]
    > Con Windows Azure puede hospedar 10 sitios Web de ASP.NET de forma gratuita y, a continuación, ajustar la escala a medida que crece el tráfico. Puede registrarse [aquí](http://aka.ms/aspnet-hol-azure).

    ![Inicie sesión en el portal de Windows Azure](aspnet-mvc-4-custom-action-filters/_static/image17.png "inicie sesión en el portal de Windows Azure")

    *Inicie sesión en el Portal de administración de Azure*
2. Haga clic en **New** en la barra de comandos.

    ![Crear un nuevo sitio Web](aspnet-mvc-4-custom-action-filters/_static/image18.png "crear un nuevo sitio Web")

    *Crear un nuevo sitio Web*
3. Haga clic en **proceso** | **sitio Web**. A continuación, seleccione **creación rápida** opción. Proporcione una dirección de URL disponible para el nuevo sitio web y haga clic en **crear sitio Web**.

    > [!NOTE]
    > Un sitio Web de Windows Azure es el host para una aplicación web que se ejecutan en la nube que puede controlar y administrar. La opción Creación rápida permite implementar una aplicación web completada al Windows Azure sitio Web desde fuera del portal. No se incluyen pasos para configurar una base de datos.

    ![Crear un nuevo sitio Web mediante Creación rápida](aspnet-mvc-4-custom-action-filters/_static/image19.png "crear un nuevo sitio Web mediante Creación rápida")

    *Crear un nuevo sitio Web mediante Creación rápida*
4. Espere hasta que el nuevo **sitio Web** se crea.
5. Una vez creado el sitio Web, haga clic en el vínculo situado bajo el **URL** columna. Compruebe que funciona el nuevo sitio Web.

    ![En el nuevo sitio web](aspnet-mvc-4-custom-action-filters/_static/image20.png "exploración al sitio web nuevo")

    *Examinar el sitio web nuevo*

    ![Sitio Web que ejecute](aspnet-mvc-4-custom-action-filters/_static/image21.png "sitio Web que se ejecuta")

    *Sitio Web que se ejecuta*
6. Vuelva al portal y haga clic en el nombre del sitio web en el **nombre** columna para mostrar las páginas de administración.

    ![Abrir las páginas de administración del sitio web](aspnet-mvc-4-custom-action-filters/_static/image22.png "abrir las páginas de administración del sitio web")

    *Abrir las páginas de administración del sitio Web*
7. En el **panel** página, en la **vista rápida** sección, haga clic en el **descargar perfil de publicación** vínculo.

    > [!NOTE]
    > El *perfil de publicación* contiene toda la información necesaria para publicar una aplicación web en un sitio Web de Windows Azure para cada método de publicación habilitado. El perfil de publicación contiene las direcciones URL, las credenciales de usuario y las cadenas de base de datos necesarias para conectarse y autenticarse en cada uno de los puntos de conexión para el que está habilitado un método de publicación. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express para Web** y **Microsoft Visual Studio 2012** admiten la lectura de perfiles de publicación para automatizar la configuración de estos programas para publicación de aplicaciones web a los sitios Web de Windows Azure.

    ![Descargar el sitio web de perfil de publicación](aspnet-mvc-4-custom-action-filters/_static/image23.png "descargar el sitio web de perfil de publicación")

    *Descargar el sitio Web de perfil de publicación*
8. Descargue el archivo de perfil de publicación en una ubicación conocida. Aún más en este ejercicio verá cómo utilizar este archivo para publicar una aplicación web a un sitios Web de Windows Azure desde Visual Studio.

    ![Guardar el archivo de perfil de publicación](aspnet-mvc-4-custom-action-filters/_static/image24.png "guardar el perfil de publicación")

    *Guardar el archivo de perfil de publicación*

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Tarea 2: configurar el servidor de base de datos

Si la aplicación realiza el uso de SQL Server debe crear un servidor de base de datos SQL de bases de datos. Si desea implementar una aplicación simple que no utiliza SQL Server, podría omitir esta tarea.

1. Necesitará un servidor de base de datos SQL para almacenar la base de datos de aplicación. Puede ver los servidores de base de datos SQL de la suscripción en el portal de administración de Windows Azure en **bases de datos Sql** | **servidores** | **del servidor Panel**. Si no tiene un servidor que creó, puede crear uno mediante la **agregar** botón en la barra de comandos. Tome nota de la **nombre del servidor y la dirección URL, nombre de inicio de sesión de administrador y contraseña**, tal y como se va a utilizar en las siguientes tareas. No cree la base de datos, tal y como se creará en una fase posterior.

    ![Panel de servidor de base de datos SQL](aspnet-mvc-4-custom-action-filters/_static/image25.png "panel de base de datos SQL Server")

    *Panel de base de datos SQL Server*
2. En la siguiente tarea probará la conexión de base de datos de Visual Studio, por ese motivo debe incluir la dirección IP local en la lista del servidor de **direcciones IP permitidas**. Para ello, haga clic en **configurar**, seleccione la dirección IP de **dirección IP del cliente actual** y péguela en el **dirección IP inicial** y **ladirecciónIPfinal** cuadros de texto y haga clic en el ![add-client-ip-address-ok-button](aspnet-mvc-4-custom-action-filters/_static/image26.png) botón.

    ![Agregar dirección IP del cliente](aspnet-mvc-4-custom-action-filters/_static/image27.png)

    *Agregar dirección IP del cliente*
3. Una vez el **dirección IP del cliente** se agrega a las direcciones IP permitidas lista, haga clic en **guardar** para confirmar los cambios.

    ![Confirmar cambios](aspnet-mvc-4-custom-action-filters/_static/image28.png)

    *Confirmar cambios*

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Tarea 3: publicar una aplicación de ASP.NET MVC 4 mediante Web Deploy

1. Vuelva a la solución de ASP.NET MVC 4. En el **el Explorador de soluciones**, haga clic en el proyecto de sitio web y seleccione **publicar**.

    ![Publicar la aplicación](aspnet-mvc-4-custom-action-filters/_static/image29.png "publicar la aplicación")

    *Publicar el sitio web*
2. Importar el perfil de publicación que se ha guardado en la primera tarea.

    ![Importar el perfil de publicación](aspnet-mvc-4-custom-action-filters/_static/image30.png "importar el perfil de publicación")

    *Importar perfil de publicación*
3. Haga clic en **validar conexión**. Una vez completada la validación, haga clic en **siguiente**.

    > [!NOTE]
    > Validación está completa cuando aparece una marca de verificación verde junto al botón Validar conexión.

    ![Validación de la conexión](aspnet-mvc-4-custom-action-filters/_static/image31.png "validación de la conexión")

    *Validación de la conexión*
4. En el **configuración** página, en la **bases de datos** sección, haga clic en el botón situado junto al cuadro de texto de la conexión de la base de datos (es decir, **DefaultConnection**).

    ![Configuración de Web deploy](aspnet-mvc-4-custom-action-filters/_static/image32.png "configuración de Web deploy")

    *Configuración de Web deploy*
5. Configure la conexión de base de datos de la manera siguiente:

   - En el **nombre del servidor** escriba la dirección URL de base de datos de SQL server mediante la *tcp:* prefijo.
   - En **nombre de usuario** escriba el nombre de inicio de sesión del Administrador de servidor.
   - En **contraseña** escriba la contraseña de inicio de sesión de administrador de servidor.
   - Escriba un nuevo nombre de base de datos.

     ![Configurar la cadena de conexión de destino](aspnet-mvc-4-custom-action-filters/_static/image33.png "configurar la cadena de conexión de destino")

     *Configurar la cadena de conexión de destino*
6. A continuación, haga clic en **Aceptar**. Cuando se le solicite para crear la base de datos, haga clic en **Sí**.

    ![Crear la base de datos](aspnet-mvc-4-custom-action-filters/_static/image34.png "crear la cadena de la base de datos")

    *Crear la base de datos*
7. La cadena de conexión que se va a usar para conectarse a la base de datos de SQL en Windows Azure se muestra en el cuadro de texto de conexión predeterminado. Después, haga clic en **Siguiente**.

    ![Cadena de conexión que apunte a la base de datos SQL](aspnet-mvc-4-custom-action-filters/_static/image35.png "cadena de conexión que apunte a la base de datos SQL")

    *Cadena de conexión que apunte a la base de datos SQL*
8. En el **vista previa** página, haga clic en **publicar**.

    ![Publicar la aplicación web](aspnet-mvc-4-custom-action-filters/_static/image36.png "publicar la aplicación web")

    *Publicar la aplicación web*
9. Una vez que finalice el proceso de publicación, el explorador predeterminado abrirá el sitio web publicado.

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a>Apéndice C: usar fragmentos de código

Con fragmentos de código, tiene todo el código que necesita a su alcance. El documento de laboratorio le indicará exactamente cuándo utilizarlas, tal como se muestra en la ilustración siguiente.

![Uso de fragmentos de código de Visual Studio para insertar código en el proyecto](aspnet-mvc-4-custom-action-filters/_static/image37.png "fragmentos de código en Visual Studio para insertar código en el proyecto")

*Uso de fragmentos de código de Visual Studio para insertar código en el proyecto*

***Para agregar un fragmento de código mediante el teclado (solo C#)***

1. Coloque el cursor donde desea insertar el código.
2. Comience a escribir el nombre del fragmento (sin espacios ni guiones).
3. Observe como coincidencia de nombres de fragmentos de código de muestra de IntelliSense.
4. Seleccione el fragmento de código correcto (o siga escribiendo hasta que se selecciona el nombre del fragmento de código completo).
5. Presione la tecla Tab dos veces para insertar el fragmento de código en la ubicación del cursor.

![Comience a escribir el nombre del fragmento](aspnet-mvc-4-custom-action-filters/_static/image38.png "comience a escribir el nombre del fragmento de código")

*Comience a escribir el nombre del fragmento de código*

![Presione la tecla Tab para seleccionar el fragmento de código resaltada](aspnet-mvc-4-custom-action-filters/_static/image39.png "presione Tab para seleccionar el fragmento de código resaltada")

*Presione la tecla Tab para seleccionar el fragmento de código resaltada*

![Vuelva a presionar Tab y el fragmento de código se expandirán](aspnet-mvc-4-custom-action-filters/_static/image40.png "vuelva a presionar Tab y el fragmento de código se expandirán")

*Vuelva a presionar Tab y el fragmento de código se expandirán*

***Para agregar un fragmento de código con el mouse (C#, en Visual Basic y XML)*** 1. Haga clic en donde desea insertar el fragmento de código.

1. Seleccione **Insertar fragmento de código** seguido **Mis fragmentos de código**.
2. Seleccione el fragmento de código relevante de la lista haciendo clic en él.

![Menú contextual donde desea insertar el fragmento de código y seleccione Insertar fragmento de código](aspnet-mvc-4-custom-action-filters/_static/image41.png "contextual donde desea insertar el fragmento de código y seleccione Insertar fragmento de código")

*Haga clic en donde desea insertar el fragmento de código y seleccione Insertar fragmento de código*

![Seleccione el fragmento de código relevante de la lista haciendo clic en él](aspnet-mvc-4-custom-action-filters/_static/image42.png "elegir el fragmento de código relevante de la lista haciendo clic en él")

*Seleccione el fragmento de código relevante de la lista haciendo clic en él*
