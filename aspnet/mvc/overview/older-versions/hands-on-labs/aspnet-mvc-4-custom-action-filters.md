---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-custom-action-filters
title: Filtros de acción personalizada de ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: ASP.NET MVC proporciona filtros de acción para ejecutar lógica de filtrado antes o después de llamar a un método de acción. Filtros de acción son atributos personalizados tha...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 969ab824-1b98-4552-81fe-b60ef5fc6887
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-custom-action-filters
msc.type: authoredcontent
ms.openlocfilehash: 879642de0bf6a9818528b847650b44e7021ca874
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37399995"
---
# <a name="aspnet-mvc-4-custom-action-filters"></a>Filtros de acción personalizada de ASP.NET MVC 4

por [campamentos Web Team](https://twitter.com/webcamps)

[Descargue el Kit de aprendizaje de campamentos de Web](https://aka.ms/webcamps-training-kit)

ASP.NET MVC proporciona filtros de acción para ejecutar lógica de filtrado antes o después de llamar a un método de acción. Los filtros de acción son atributos personalizados que proporcionan un medio declarativo para agregar comportamiento previo y posterior a la acción a los métodos de acción del controlador.

En este laboratorio práctico creará un atributo de filtro de acción personalizada en solución MvcMusicStore para capturar las solicitudes del controlador y registrar la actividad de un sitio en una tabla de base de datos. Podrá agregar el filtro de registro por inyección de código a cualquier acción o controlador. Por último, verá la vista del registro que muestra la lista de los visitantes.

Esta práctica se da por supuesto que tiene conocimientos básicos de **ASP.NET MVC**. Si no ha usado **ASP.NET MVC** antes, le recomendamos que repase **aspectos básicos de ASP.NET MVC 4** laboratorios prácticos.

> [!NOTE]
> Todo el código de ejemplo y fragmentos de código se incluyen en el Kit de aprendizaje de Web campamentos, disponible desde en [versiones de Microsoft de Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit). Está disponible en el proyecto específico para este laboratorio [filtros de acción personalizada de ASP.NET MVC 4](https://github.com/Microsoft-Web/HOL-MVC4CustomActionFilters).

<a id="Objectives"></a>
### <a name="objectives"></a>Objetivos

En este laboratorio práctico, aprenderá cómo:

- Crear un atributo de filtro de acción personalizada para ampliar las capacidades de filtrado
- Aplicar un atributo de filtro personalizado mediante la inserción de un nivel específico
- Registrar globalmente los filtros de acción personalizada

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

Si no está familiarizado con los fragmentos de código de Visual Studio y desea aprender a usarlas, puede consultar el apéndice de este documento &quot; [Apéndice C: Using Code Snippets](#AppendixC)&quot;.

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Ejercicios

Esta práctica se compone de los ejercicios siguientes:

1. [Ejercicio 1: Registrar acciones](#Exercise1)
2. [Ejercicio 2: Administración de varios filtros de acción](#Exercise2)

Tiempo estimado para completar esta práctica: **30 minutos**.

> [!NOTE]
> Cada ejercicio está acompañado por un **final** carpeta que contiene la solución resultante debe obtener después de completar los ejercicios. Puede usar esta solución como una guía si necesita ayuda adicional para trabajar a través de los ejercicios.


<a id="Exercise1"></a>

<a id="Exercise_1_Logging_Actions"></a>
### <a name="exercise-1-logging-actions"></a>Ejercicio 1: Registrar acciones

En este ejercicio, obtendrá información sobre cómo crear un filtro de acción personalizada de registro mediante el uso de proveedores de filtros de ASP.NET MVC 4. Para ello aplicará un filtro de registro para el sitio de Music Store tal que registrará todas las actividades en los controladores seleccionados.

El filtro se extenderá **ActionFilterAttributeClass** e invalidar **OnActionExecuting** método para detectar cada solicitud y, a continuación, realice las acciones de registro. Ejecutar métodos, los resultados y parámetros se proporcionará información de contexto acerca de las solicitudes HTTP, ASP.NET MVC **ActionExecutingContext** clase **.**

> [!NOTE]
> ASP.NET MVC 4 también tiene proveedores de filtros predeterminados que puede utilizar sin necesidad de crear un filtro personalizado. ASP.NET MVC 4 proporciona los siguientes tipos de filtros:
> 
> - **Autorización** filtrar, que toma decisiones de seguridad sobre si se debe ejecutar un método de acción, como realizar la autenticación o validar las propiedades de la solicitud.
> - **Acción** filtro, que encapsula la ejecución del método de acción. Este filtro puede llevar a cabo procesos adicionales, como proporcionar datos extra al método de acción, inspeccionar el valor devuelto o cancelar la ejecución del método de acción
> - **Resultado** filtro, que encapsula la ejecución del objeto ActionResult. Este filtro puede realizar un procesamiento adicional del resultado, como modificar la respuesta HTTP.
> - **Excepción** filtro, que se ejecuta si hay una excepción no controlada en algún lugar en el método de acción, empezando por los filtros de autorización y finalizando con la ejecución del resultado. Los filtros de excepciones pueden usarse para tareas como registrar o mostrar una página de error.
> 
> Para obtener más información acerca de los proveedores de filtros, visite este vínculo de MSDN: ([https://msdn.microsoft.com/library/dd410209.aspx](https://msdn.microsoft.com/library/dd410209.aspx)).


<a id="AboutLoggingFeature"></a>

<a id="About_MVC_Music_Store_Application_logging_feature"></a>
#### <a name="about-mvc-music-store-application-logging-feature"></a>Acerca de la característica de registro de aplicación de MVC Music Store

Esta solución de Music Store tiene una nueva tabla de modelo de datos para el registro de sitio, **ActionLog**, con los siguientes campos: nombre del controlador que recibió una solicitud, acción de llamada, IP de cliente y marca de tiempo.

![Modelo de datos. Tabla ActionLog. ](aspnet-mvc-4-custom-action-filters/_static/image1.png "Modelo de datos. Tabla ActionLog.")

*Modelo de datos: tabla ActionLog*

La solución proporciona una vista de MVC de ASP.NET para el registro de acciones que puede encontrarse en **MvcMusicStores/vistas/ActionLog**:

![Vista de registro de acción](aspnet-mvc-4-custom-action-filters/_static/image2.png "Ver registro de acciones")

*Vista de registro de acción*

Con esta estructura, todo el trabajo se centrará en la solicitud del controlador de interrupción y realizar el registro mediante el filtrado personalizado.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_a_Custom_Filter_to_Catch_a_Controllers_Request"></a>
#### <a name="task-1---creating-a-custom-filter-to-catch-a-controllers-request"></a>Tarea 1: crear un filtro personalizado para detectar solicitud de un controlador

En esta tarea creará una clase de atributo de filtro personalizado que contendrá la lógica de registro. Para ello podrá ampliar ASP.NET MVC **ActionFilterAttribute** clase e implemente la interfaz **IActionFilter**.

> [!NOTE]
> El **ActionFilterAttribute** es la clase base para todos los filtros de atributo. Proporciona los métodos siguientes para ejecutar una lógica específica después y antes de la ejecución de la acción de controlador:
> 
> - **OnActionExecuting**(ActionExecutingContext filterContext): justo antes de la acción se llama al método.
> - **OnActionExecuted**(ActionExecutedContext filterContext): una vez que se llama al método de acción y antes de ejecutar el resultado (antes de la representación de vista).
> - **OnResultExecuting**(ActionExecutingContext filterContext): justo antes de ejecutar el resultado (antes de la representación de vista).
> - **OnResultExecuted**(ResultExecutedContext filterContext): después de ejecuta el resultado (después de representa la vista).
> 
> Al reemplazar cualquiera de estos métodos en una clase derivada, puede ejecutar su propio código de filtrado.


1. Abra el **comenzar** solución ubicado en **\Source\Ex01-LoggingActions\Begin** carpeta.

   1. Deberá descargar algunos paquetes de NuGet que faltan antes de continuar. Para ello, haga clic en el **proyecto** menú y seleccione **administrar paquetes de NuGet**.
   2. En el **administrar paquetes de NuGet** cuadro de diálogo, haga clic en **restaurar** para descargar los paquetes que falten.
   3. Por último, compile la solución haciendo **compilar** | **compilar solución**.

      > [!NOTE]
      > Una de las ventajas de usar NuGet es que no tienen que enviar todas las bibliotecas en el proyecto, lo que reduce el tamaño del proyecto. Con las herramientas avanzadas de NuGet, mediante la especificación de las versiones del paquete en el archivo Packages.config, podrá descargar todas las bibliotecas requeridas en la primera vez que ejecute el proyecto. Esto es por eso tendrá que ejecutar estos pasos después de abrir una solución existente de este laboratorio.
      > 
      > Para obtener más información, consulte este artículo: [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages ](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).
2. Agregue una nueva clase de C# en el **filtros** carpeta y asígnele el nombre *CustomActionFilter.cs*. Esta carpeta almacenará todos los filtros personalizados.
3. Abra **CustomActionFilter.cs** y agregue una referencia a **System.Web.Mvc** y **MvcMusicStore.Models** espacios de nombres:

    (Código de fragmento de código - *filtros de acción personalizada de ASP.NET MVC 4 - Ex1 CustomActionFilterNamespaces*)

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample1.cs)]
4. Heredar la **filtro CustomActionFilter** clase **ActionFilterAttribute** y, a continuación, realice **filtro CustomActionFilter** implementan **IActionFilter** interfaz.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample2.cs)]
5. Asegúrese de **filtro CustomActionFilter** clase invalide el método **OnActionExecuting** y agregue la lógica necesaria para registrar la ejecución del filtro. Para ello, agregue el siguiente código resaltado en **filtro CustomActionFilter** clase.

    (Código de fragmento de código - *filtros de acción personalizada de ASP.NET MVC 4 - Ex1 LoggingActions*)

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample3.cs#Highlight)]

    > [!NOTE]
    > **OnActionExecuting** está usando el método **Entity Framework** para agregar un nuevo registro ActionLog. Crea y rellena una nueva instancia de entidad con la información de contexto de **filterContext**.
    > 
    > Puede leer más sobre **elemento ControllerContext** en la clase [msdn](https://msdn.microsoft.com/library/system.web.mvc.controllercontext.aspx).

<a id="Ex1Task2"></a>

<a id="Task_2_-_Injecting_a_Code_Interceptor_into_the_Store_Controller_Class"></a>
#### <a name="task-2---injecting-a-code-interceptor-into-the-store-controller-class"></a>Tarea 2: inserta un Interceptor de código en la clase de controlador de Store

En esta tarea agregará el filtro personalizado insertando a todas las clases de controlador y las acciones de controlador que se registrarán. En este ejercicio, la clase de controlador Store tendrá un registro.

El método **OnActionExecuting** desde **ActionLogFilterAttribute** filtro personalizado se ejecuta cuando se llama a un elemento insertado.

También es posible interceptar un método de controlador específico.

1. Abra el **StoreController** en **MvcMusicStore\Controllers** y agregue una referencia a la **filtros** espacio de nombres:

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample4.cs)]
2. Insertar el filtro personalizado **filtro CustomActionFilter** en **StoreController** clase agregando **[filtro CustomActionFilter]** atributo antes de la declaración de clase.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample5.cs)]

   > [!NOTE]
   > Cuando se inserta un filtro en una clase de controlador, también se insertan todas sus acciones. Si desea aplicar el filtro solo para un conjunto de acciones, tendría que insertar **[filtro CustomActionFilter]** a cada uno de ellos:
   > 
   > [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample6.cs)]

<a id="Ex1Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Tarea 3: ejecutar la aplicación

En esta tarea, probará que funciona el filtro del registro. Se iniciará la aplicación y visitar la tienda y, a continuación, comprobará las actividades registradas.

1. Presione **F5** para ejecutar la aplicación.
2. Vaya a **/ActionLog** para ver el estado inicial de la vista de registro:

    ![Seguimiento de estado antes de la actividad de la página del registro](aspnet-mvc-4-custom-action-filters/_static/image3.png "tracker estado antes de la actividad de la página de registro")

    *Estado de seguimiento del registro antes de la actividad de la página*

   > [!NOTE]
   > De forma predeterminada, siempre mostrará un elemento que se genera cuando se recuperan los géneros existentes para el menú.
   > 
   > Por motivos de simplicidad estamos limpiando el **ActionLog** tabla cada vez que se ejecuta la aplicación, por lo que sólo mostrará los registros de comprobación de cada tarea determinada.
   > 
   > Es posible que deba quitar el código siguiente desde el **sesión\_iniciar** método (en el **Global.asax** clase), con el fin de guardar un registro histórico de todas las acciones que se ejecuta en el Store Controlador.
   > 
   > [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample7.cs)]
3. Haga clic en uno de los **géneros** en el menú y realizar algunas acciones, como navegar por un álbum disponible.
4. Vaya a **/ActionLog** y si el registro está vacío presione **F5** para actualizar la página. Compruebe que se realizara el seguimiento de las visitas:

    ![Registro de acciones con el registro de actividad](aspnet-mvc-4-custom-action-filters/_static/image4.png "registro de acciones con el registro de actividad")

    *Registro de acciones con el registro de actividad*

<a id="Exercise2"></a>

<a id="Exercise_2_Managing_Multiple_Action_Filters"></a>
### <a name="exercise-2-managing-multiple-action-filters"></a>Ejercicio 2: Administración de varios filtros de acción

En este ejercicio agregará un segundo filtro de acción personalizado a la clase StoreController y definir el orden específico en el que se ejecutarán ambos filtros. A continuación, se actualizará el código para registrar el filtro global.

Existen diferentes opciones para tener en cuenta al definir el orden de ejecución de los filtros. Por ejemplo, la propiedad orden y ámbito de los filtros:

Puede definir un **ámbito** para cada uno de los filtros, por ejemplo, podría definir el ámbito todos los filtros de acción para ejecutarse en el **controlador ámbito**y todos los filtros de autorización para ejecutarse en **ámbito Global** . Los ámbitos tienen un orden de ejecución definido.

Además, cada filtro de acción tiene una propiedad de orden que se usa para determinar el orden de ejecución en el ámbito del filtro.

Para obtener más información sobre el orden de ejecución de los filtros de acción personalizado, visite este artículo de MSDN: ([https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx](https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx)).

<a id="Ex2Task1"></a>

<a id="Task_1_Creating_a_new_Custom_Action_Filter"></a>
#### <a name="task-1-creating-a-new-custom-action-filter"></a>Tarea 1: Crear un nuevo filtro de acción personalizado

En esta tarea, creará un nuevo filtro de acción personalizado para insertar en la clase StoreController, aprender a administrar el orden de ejecución de los filtros.

1. Abra el **comenzar** solución ubicado en **\Source\Ex02-ManagingMultipleActionFilters\Begin** carpeta. En caso contrario, es posible que siga usando la **final** solución obtenido completando el ejercicio anterior.

    1. Si abrió proporcionado **comenzar** solución, deberá descargar algunos paquetes de NuGet que faltan antes de continuar. Para ello, haga clic en el **proyecto** menú y seleccione **administrar paquetes de NuGet**.
    2. En el **administrar paquetes de NuGet** cuadro de diálogo, haga clic en **restaurar** para descargar los paquetes que falten.
    3. Por último, compile la solución haciendo **compilar** | **compilar solución**.

        > [!NOTE]
        > Una de las ventajas de usar NuGet es que no tienen que enviar todas las bibliotecas en el proyecto, lo que reduce el tamaño del proyecto. Con las herramientas avanzadas de NuGet, mediante la especificación de las versiones del paquete en el archivo Packages.config, podrá descargar todas las bibliotecas requeridas en la primera vez que ejecute el proyecto. Esto es por eso tendrá que ejecutar estos pasos después de abrir una solución existente de este laboratorio.
        > 
        > Para obtener más información, consulte este artículo: [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages ](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).
2. Agregue una nueva clase de C# en el **filtros** carpeta y asígnele el nombre *MyNewCustomActionFilter.cs*
3. Abra **MyNewCustomActionFilter.cs** y agregue una referencia a **System.Web.Mvc** y **MvcMusicStore.Models** espacio de nombres:

    (Código de fragmento de código - *filtros de acción personalizada de ASP.NET MVC 4 - Ex2 MyNewCustomActionFilterNamespaces*)

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample8.cs)]
4. Reemplace la declaración de clase predeterminado con el código siguiente.

    (Código de fragmento de código - *filtros de acción personalizada de ASP.NET MVC 4 - Ex2 MyNewCustomActionFilterClass*)

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample9.cs)]

    > [!NOTE]
    > Este filtro de acción personalizado es prácticamente el mismo que el que creó en el ejercicio anterior. La principal diferencia es que tiene el *&quot;iniciado por&quot;* atributo actualizado con el nombre de esta nueva clase para identificar el filtro wich registra el registro.

<a id="Ex2Task2"></a>

<a id="Task_2_Injecting_a_new_Code_Interceptor_into_the_StoreController_Class"></a>
#### <a name="task-2-injecting-a-new-code-interceptor-into-the-storecontroller-class"></a>Tarea 2: Inserta un nuevo Interceptor de código en la clase StoreController

En esta tarea, agregará un nuevo filtro personalizado en la clase StoreController y ejecutar la solución para comprobar cómo estos dos filtros funcionan juntos.

1. Abra el **StoreController** clase ubicado en **MvcMusicStore\Controllers** e insertar el nuevo filtro personalizado **MyNewCustomActionFilter** en  **StoreController** clase, como se muestra en el código siguiente.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample10.cs)]
2. Ahora, ejecute la aplicación con el fin de ver cómo funcionan estos dos filtros de acción personalizada. Para ello, presione **F5** y espere hasta que se inicia la aplicación.
3. Vaya a **/ActionLog** para ver el estado inicial de la vista de registro.

    ![Seguimiento de estado antes de la actividad de la página del registro](aspnet-mvc-4-custom-action-filters/_static/image5.png "tracker estado antes de la actividad de la página de registro")

    *Estado de seguimiento del registro antes de la actividad de la página*
4. Haga clic en uno de los **géneros** en el menú y realizar algunas acciones, como navegar por un álbum disponible.
5. Compruebe que esta vez; las visitas se realizara el seguimiento dos veces: una vez para cada uno de los filtros de acción personalizado que agregó en el **la controladora de almacenamiento** clase.

    ![Registro de acciones con el registro de actividad](aspnet-mvc-4-custom-action-filters/_static/image6.png "registro de acciones con el registro de actividad")

    *Registro de acciones con el registro de actividad*
6. Cierre el explorador.

<a id="Ex2Task3"></a>

<a id="Task_3_Managing_Filter_Ordering"></a>
#### <a name="task-3-managing-filter-ordering"></a>Tarea 3: Administrar el orden de los filtros

En esta tarea, obtendrá información sobre cómo administrar el orden de ejecución de los filtros utilizando la propiedad de orden.

1. Abra el **StoreController** clase ubicado en **MvcMusicStore\Controllers** y especifique el **orden** como propiedad en ambos filtros se muestra a continuación.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample11.cs)]
2. Ahora, para comprobar cómo los filtros se ejecutan según el valor de la propiedad de su pedido. Observará que el filtro con el menor valor de orden (**filtro CustomActionFilter**) es la primera que se ejecuta. Presione **F5** y espere hasta que se inicia la aplicación.
3. Vaya a **/ActionLog** para ver el estado inicial de la vista de registro.

    ![Seguimiento de estado antes de la actividad de la página del registro](aspnet-mvc-4-custom-action-filters/_static/image7.png "tracker estado antes de la actividad de la página de registro")

    *Estado de seguimiento del registro antes de la actividad de la página*
4. Haga clic en uno de los **géneros** en el menú y realizar algunas acciones, como navegar por un álbum disponible.
5. Comprobación de que esta vez, las visitas se realizara el seguimiento se ordenan por valor de orden de los filtros: **filtro CustomActionFilter** registros primero.

    ![Registro de acciones con el registro de actividad](aspnet-mvc-4-custom-action-filters/_static/image8.png "registro de acciones con el registro de actividad")

    *Registro de acciones con el registro de actividad*
6. Ahora, va a actualizar el valor de orden de los filtros y comprobar cómo se cambia el orden de registro. En el **StoreController** class, actualice el valor de orden de los filtros como se muestra a continuación.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample12.cs)]
7. Vuelva a ejecutar la aplicación presionando **F5**.
8. Haga clic en uno de los **géneros** en el menú y realizar algunas acciones, como navegar por un álbum disponible.
9. Compruebe que esta vez, los registros crean por **MyNewCustomActionFilter** filtro aparece en primer lugar.

    ![Registro de acciones con el registro de actividad](aspnet-mvc-4-custom-action-filters/_static/image9.png "registro de acciones con el registro de actividad")

    *Registro de acciones con el registro de actividad*

<a id="Ex2Task4"></a>

<a id="Task_4_Registering_Filters_Globally"></a>
#### <a name="task-4-registering-filters-globally"></a>Tarea 4: Registro de filtros global

En esta tarea, actualizará la solución para registrar el nuevo filtro (**MyNewCustomActionFilter**) como un filtro global. Al hacerlo, se desencadena por todas las acciones realizan las operaciones en la aplicación y no solo en la otras StoreController como se muestra en la tarea anterior.

1. En **StoreController** class, quitar **[MyNewCustomActionFilter]** atributo y la propiedad order de **[filtro CustomActionFilter]**. Debería ser similar al siguiente:

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample13.cs)]
2. Abra **Global.asax** de archivo y busque el **aplicación\_iniciar** método. Tenga en cuenta que cada vez que inicia la aplicación se registra mediante una llamada a los filtros globales **RegisterGlobalFilters** método dentro de **FilterConfig** clase.

    ![Registrar los filtros globales en Global.asax](aspnet-mvc-4-custom-action-filters/_static/image10.png "registrar filtros globales en Global.asax")

    *Registrar los filtros globales en Global.asax*
3. Abra **FilterConfig.cs** dentro de archivos **aplicación\_iniciar** carpeta.
4. Agregue una referencia al uso de System.Web.Mvc; uso de MvcMusicStore.Filters; espacio de nombres.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample14.cs)]
5. Actualización **RegisterGlobalFilters** método agregar el filtro personalizado. Para ello, agregue el código resaltado:

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample15.cs)]
6. Ejecute la aplicación presionando **F5**.
7. Haga clic en uno de los **géneros** en el menú y realizar algunas acciones, como navegar por un álbum disponible.
8. Compruebe que ahora **[MyNewCustomActionFilter]** es que se va a insertar en HomeController y ActionLogController demasiado.

    ![Registro de acciones con el registro de actividad](aspnet-mvc-4-custom-action-filters/_static/image11.png "registro de acciones con el registro de actividad")

    *Registro de acciones con el registro de actividad global*

> [!NOTE]
> Además, puede implementar esta aplicación a los siguientes sitios Web Windows Azure [Apéndice B: publicar una aplicación ASP.NET MVC 4 mediante Web Deploy](#AppendixB).


* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Resumen

Al completar este laboratorio práctico ha aprendido cómo ampliar un filtro de acción para ejecutar acciones personalizadas. También ha aprendido cómo inyectar cualquier filtro a los controladores de página. Se usaron los conceptos siguientes:

- Cómo crear filtros de acción personalizado con la clase ActionFilterAttribute de MVC de ASP.NET
- Cómo insertar filtros en los controladores ASP.NET MVC
- Cómo administrar el orden de los filtros utilizando la propiedad Order
- Cómo registrar filtros global

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Apéndice A: instalación de Visual Studio Express 2012 para Web

Puede instalar **Microsoft Visual Studio Express 2012 para Web** u otro &quot;Express&quot; versión utilizando el **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**. Las instrucciones siguientes le guían a través de los pasos necesarios para instalar *Visual studio Express 2012 para Web* mediante *Microsoft Web Platform Installer*.

1. Vaya a [ [ https://go.microsoft.com/? linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). Como alternativa, si ya ha instalado el instalador de plataforma Web, puede abrirla y busque el producto &quot; <em>Visual Studio Express 2012 for Web con SDK de Windows Azure</em>&quot;.
2. Haga clic en **instalar ahora**. Si no tienes **instalador de plataforma Web** se le redirigirá para descargarlo e instalarlo en primer lugar.
3. Una vez **instalador de plataforma Web** está abierto, haga clic en **instalar** para iniciar el programa de instalación.

    ![Instale Visual Studio Express](aspnet-mvc-4-custom-action-filters/_static/image12.png "instale Visual Studio Express")

    *Instale Visual Studio Express*
4. Leer todos los productos las licencias y los términos y haga clic en **acepto** para continuar.

    ![Acepte los términos de licencia](aspnet-mvc-4-custom-action-filters/_static/image13.png)

    *Acepte los términos de licencia*
5. Espere hasta que finalice el proceso de descarga e instalación.

    ![Progreso de la instalación](aspnet-mvc-4-custom-action-filters/_static/image14.png)

    *Progreso de la instalación*
6. Cuando se complete la instalación, haga clic en **finalizar**.

    ![Instalación completada](aspnet-mvc-4-custom-action-filters/_static/image15.png)

    *Instalación completada*
7. Haga clic en **Exit** para cerrar el instalador de plataforma Web.
8. Para abrir Visual Studio Express para Web, vaya a la **iniciar** pantalla y comienza a escribir &quot; **VS Express**&quot;, a continuación, haga clic en el **VS Express para Web** icono.

    ![VS Express para el icono de Web](aspnet-mvc-4-custom-action-filters/_static/image16.png)

    *VS Express para el icono de Web*

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Apéndice B: publicar una aplicación ASP.NET MVC 4 mediante Web Deploy

En este apéndice se mostrará cómo crear un nuevo sitio web desde el Portal de administración de Windows Azure y publicar la aplicación que obtuvo siguiendo el laboratorio, que aprovecha la característica de publicación Web Deploy proporcionada por Windows Azure.

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a>Tarea 1: crear un nuevo sitio Web de Windows Azure Portal

1. Vaya a la [Portal de administración de Windows Azure](https://manage.windowsazure.com/) e inicie sesión con las credenciales de Microsoft asociadas con su suscripción.

    > [!NOTE]
    > Con Windows Azure puede hospedar 10 sitios Web ASP.NET de forma gratuita y, a continuación, escalar a medida que crece el tráfico. Puede registrarse [aquí](http://aka.ms/aspnet-hol-azure).

    ![Inicie sesión en el portal de Windows Azure](aspnet-mvc-4-custom-action-filters/_static/image17.png "inicie sesión en el portal de Windows Azure")

    *Inicie sesión en el Portal de administración de Azure de Windows*
2. Haga clic en **New** en la barra de comandos.

    ![Crear un nuevo sitio Web](aspnet-mvc-4-custom-action-filters/_static/image18.png "crear un nuevo sitio Web")

    *Crear un nuevo sitio Web*
3. Haga clic en **proceso** | **sitio Web**. A continuación, seleccione **creación rápida** opción. Proporcione una dirección URL disponible para el nuevo sitio web y haga clic en **crear sitio Web**.

    > [!NOTE]
    > Un sitio Web de Windows Azure es el host para una aplicación web que se ejecutan en la nube que puede controlar y administrar. La opción Creación rápida permite implementar una aplicación web completada en el sitio Web Windows Azure desde fuera del portal. No incluye los pasos para configurar una base de datos.

    ![Crear un nuevo sitio Web mediante Creación rápida](aspnet-mvc-4-custom-action-filters/_static/image19.png "crear un nuevo sitio Web mediante Creación rápida")

    *Crear un nuevo sitio Web mediante Creación rápida*
4. Espere hasta que el nuevo **sitio Web** se crea.
5. Una vez creado el sitio Web, haga clic en el vínculo situado bajo el **URL** columna. Compruebe que funciona el nuevo sitio Web.

    ![Exploración al sitio web nuevo](aspnet-mvc-4-custom-action-filters/_static/image20.png "exploración al sitio web nuevo")

    *Exploración al sitio web nuevo*

    ![Sitio Web que se ejecuta](aspnet-mvc-4-custom-action-filters/_static/image21.png "sitio Web que se ejecuta")

    *Sitio Web que se ejecuta*
6. Vuelva al portal y haga clic en el nombre del sitio web en el **nombre** columna para mostrar las páginas de administración.

    ![Abrir las páginas de administración del sitio web](aspnet-mvc-4-custom-action-filters/_static/image22.png "abrir las páginas de administración del sitio web")

    *Abrir las páginas de administración del sitio Web*
7. En el **panel** página, en el **primea** sección, haga clic en el **descargar perfil de publicación** vínculo.

    > [!NOTE]
    > El *perfil de publicación* contiene toda la información necesaria para publicar una aplicación web en un sitio Web de Windows Azure para cada método de publicación habilitado. El perfil de publicación contiene las direcciones URL, las credenciales de usuario y las cadenas de base de datos necesarias para conectarse y autenticarse en cada uno de los puntos de conexión para el que está habilitado un método de publicación. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express para Web** y **Microsoft Visual Studio 2012** admiten la lectura de perfiles de publicación para automatizar la configuración de estos programas para publicación de aplicaciones web para sitios Web de Windows Azure.

    ![Descargando el sitio web de perfil de publicación](aspnet-mvc-4-custom-action-filters/_static/image23.png "descargando el sitio web de perfil de publicación")

    *Descargando el sitio Web de perfil de publicación*
8. Descargue el archivo de perfil de publicación en una ubicación conocida. Aún más en este ejercicio verá cómo usar este archivo para publicar una aplicación web a un sitios Web de Windows Azure desde Visual Studio.

    ![Guardar el archivo de perfil de publicación](aspnet-mvc-4-custom-action-filters/_static/image24.png "guardar el perfil de publicación")

    *Guardar el archivo de perfil de publicación*

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Tarea 2: configurar el servidor de base de datos

Si la aplicación hace uso de SQL Server deberá crear un servidor de base de datos SQL de bases de datos. Si desea implementar una aplicación simple que no utiliza SQL Server, podría omitir esta tarea.

1. Necesitará un servidor de base de datos SQL para almacenar la base de datos de aplicación. Puede ver los servidores de base de datos SQL de la suscripción en el portal de administración de Windows Azure en **bases de datos Sql** | **servidores** | **del servidor Panel**. Si no tiene un servidor creado, puede crear una mediante el **agregar** botón en la barra de comandos. Tome nota de la **nombre del servidor y dirección URL, nombre de inicio de sesión de administrador y contraseña**, ya que lo usará en las siguientes tareas. No cree la base de datos, como se crearán en una etapa posterior.

    ![Panel de base de datos SQL Server](aspnet-mvc-4-custom-action-filters/_static/image25.png "panel de base de datos SQL Server")

    *Panel de base de datos SQL Server*
2. En la siguiente tarea probará la conexión de base de datos desde Visual Studio, por ese motivo debe incluir la dirección IP local en la lista del servidor de **direcciones IP permitidas**. Para ello, haga clic en **configurar**, seleccione la dirección IP de **dirección IP del cliente actual** y péguelo en el **dirección IP inicial** y **ladirecciónIPfinal** cuadros de texto y haga clic en el ![add-client-ip-address-ok-button](aspnet-mvc-4-custom-action-filters/_static/image26.png) botón.

    ![Agregar dirección IP del cliente](aspnet-mvc-4-custom-action-filters/_static/image27.png)

    *Agregar dirección IP del cliente*
3. Una vez el **dirección IP del cliente** se agrega a las direcciones IP permitidas de lista, haga clic en **guardar** para confirmar los cambios.

    ![Confirmar cambios](aspnet-mvc-4-custom-action-filters/_static/image28.png)

    *Confirmar cambios*

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Tarea 3: publicar una aplicación de ASP.NET MVC 4 mediante Web Deploy

1. Vuelva a la solución de ASP.NET MVC 4. En el **el Explorador de soluciones**, haga clic en el proyecto de sitio web y seleccione **publicar**.

    ![Publicar la aplicación](aspnet-mvc-4-custom-action-filters/_static/image29.png "publicar la aplicación")

    *Publicar el sitio web*
2. Importar el perfil de publicación que guardó en la primera tarea.

    ![Importar el perfil de publicación](aspnet-mvc-4-custom-action-filters/_static/image30.png "importar el perfil de publicación")

    *Importar perfil de publicación*
3. Haga clic en **validar conexión**. Una vez completada la validación, haga clic en **siguiente**.

    > [!NOTE]
    > Validación está completa cuando aparece una marca de verificación verde junto al botón Validar conexión.

    ![Validación de la conexión](aspnet-mvc-4-custom-action-filters/_static/image31.png "validación de la conexión")

    *Validación de la conexión*
4. En el **configuración** página, en el **bases de datos** sección, haga clic en el botón situado junto al cuadro de texto de la conexión de base de datos (es decir, **DefaultConnection**).

    ![Configuración de implementación Web](aspnet-mvc-4-custom-action-filters/_static/image32.png "configuración de implementación Web")

    *Configuración de implementación Web*
5. Configure la conexión de base de datos como sigue:

   - En el **nombre del servidor** escriba la dirección URL de base de datos de SQL server mediante el *tcp:* prefijo.
   - En **nombre de usuario** escriba el nombre de inicio de sesión del Administrador de servidor.
   - En **contraseña** escriba la contraseña de inicio de sesión del Administrador de servidor.
   - Escriba un nuevo nombre de base de datos.

     ![Configuración de la cadena de conexión de destino](aspnet-mvc-4-custom-action-filters/_static/image33.png "configuración de la cadena de conexión de destino")

     *Configuración de la cadena de conexión de destino*
6. A continuación, haga clic en **Aceptar**. Cuando se le pedirá que cree la base de datos, haga clic en **Sí**.

    ![Creación de la base de datos](aspnet-mvc-4-custom-action-filters/_static/image34.png "creación de la cadena de la base de datos")

    *Creación de la base de datos*
7. La cadena de conexión que se usará para conectarse a la base de datos de SQL en Windows Azure se muestra en el cuadro de texto de conexión predeterminado. Después, haga clic en **Siguiente**.

    ![Cadena de conexión que apunte a la base de datos SQL](aspnet-mvc-4-custom-action-filters/_static/image35.png "cadena de conexión que apunte a la base de datos SQL")

    *Cadena de conexión que apunte a la base de datos SQL*
8. En el **Preview** página, haga clic en **publicar**.

    ![Publicar la aplicación web](aspnet-mvc-4-custom-action-filters/_static/image36.png "publicar la aplicación web")

    *Publicar la aplicación web*
9. Una vez que finalice el proceso de publicación, el explorador predeterminado abrirá el sitio web publicado.

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a>Apéndice C: uso de fragmentos de código

Con fragmentos de código, tiene todo el código que necesita a su alcance. El documento de laboratorio le dirá exactamente cuándo se pueden utilizar, como se muestra en la ilustración siguiente.

![Uso de fragmentos de código de Visual Studio para insertar código en el proyecto](aspnet-mvc-4-custom-action-filters/_static/image37.png "fragmentos de código en Visual Studio para insertar código en el proyecto")

*Uso de fragmentos de código de Visual Studio para insertar código en el proyecto*

***Para agregar un fragmento de código mediante el teclado (solo C#)***

1. Coloque el cursor donde desea insertar el código.
2. Comience a escribir el nombre del fragmento de código (sin espacios ni guiones).
3. Observe cómo IntelliSense muestra los nombres de fragmentos de código coincidentes.
4. Seleccione el fragmento de código correcto (o siga escribiendo hasta que se selecciona el nombre del fragmento de código completo).
5. Presione la tecla Tab dos veces para insertar el fragmento de código en la ubicación del cursor.

![Comience a escribir el nombre del fragmento](aspnet-mvc-4-custom-action-filters/_static/image38.png "comience a escribir el nombre del fragmento de código")

*Comience a escribir el nombre del fragmento de código*

![Presione la tecla Tab para seleccionar el fragmento de código resaltada](aspnet-mvc-4-custom-action-filters/_static/image39.png "presione Tab para seleccionar el fragmento de código resaltada")

*Presione la tecla Tab para seleccionar el fragmento de código resaltada*

![Vuelva a presionar Tab y el fragmento de código se expandirán](aspnet-mvc-4-custom-action-filters/_static/image40.png "vuelva a presionar Tab y el fragmento de código se expandirán")

*Vuelva a presionar Tab y el fragmento de código se expandirán*

***Para agregar un fragmento de código con el mouse (C#, en Visual Basic y XML)*** 1. Haga doble clic en el desea insertar el fragmento de código.

1. Seleccione **Insertar fragmento de código** seguido **Mis fragmentos de código**.
2. Seleccione el fragmento de código relevante en la lista, haciendo clic en él.

![Con el botón secundario donde desea insertar el fragmento de código y seleccione Insertar fragmento de código](aspnet-mvc-4-custom-action-filters/_static/image41.png "contextual donde desea insertar el fragmento de código y seleccione Insertar fragmento de código")

*Haga doble clic donde desea insertar el fragmento de código y seleccione Insertar fragmento de código*

![Elegir el fragmento de código relevante en la lista, hacer clic en ella](aspnet-mvc-4-custom-action-filters/_static/image42.png "elegir el fragmento de código relevante en la lista, hacer clic en ella")

*Elegir el fragmento de código relevante en la lista, hacer clic en ella*
