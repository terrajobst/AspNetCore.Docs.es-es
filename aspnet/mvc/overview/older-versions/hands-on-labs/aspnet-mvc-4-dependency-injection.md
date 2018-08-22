---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
title: Inserción de dependencias de ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: 'Nota: Esta práctica se supone que tiene conocimientos básicos de los filtros de MVC de ASP.NET y ASP.NET MVC 4. Si no ha usado los filtros de ASP.NET MVC 4 antes, se rec...'
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 84c7baca-1c54-4c44-8f52-4282122d6acb
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 3f9222c7b485f552da91f4875c882db7e03cdd0a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827792"
---
# <a name="aspnet-mvc-4-dependency-injection"></a>Inserción de dependencias de ASP.NET MVC 4

por [campamentos Web Team](https://twitter.com/webcamps)

[Descargue el Kit de aprendizaje de campamentos de Web](https://aka.ms/webcamps-training-kit)

Esta práctica se da por supuesto que tiene conocimientos básicos de **ASP.NET MVC** y **filtros de ASP.NET MVC 4**. Si no ha usado **filtros de ASP.NET MVC 4** antes, le recomendamos que repase **filtros de acción personalizada de ASP.NET MVC** laboratorios prácticos.

> [!NOTE]
> Todo el código de ejemplo y fragmentos de código se incluyen en el Kit de aprendizaje de Web campamentos, disponible desde en [versiones de Microsoft de Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit). Está disponible en el proyecto específico para este laboratorio [inserción de dependencias de ASP.NET MVC 4](https://github.com/Microsoft-Web/HOL-MVC4DependencyInjection).

En **programación orientada a objetos** paradigma, objetos trabajan juntos en un modelo de colaboración donde hay colaboradores y los consumidores. Naturalmente, este modelo de comunicación genera las dependencias entre objetos y componentes, cada vez son difíciles de administrar cuando aumenta la complejidad.

![Clase dependencias y la complejidad del modelo](aspnet-mvc-4-dependency-injection/_static/image1.png "clase dependencias y la complejidad del modelo")

*Dependencias de clase y la complejidad de modelo*

Probablemente habrá oído hablar acerca de la **patrón Factory** y la separación entre la interfaz y la implementación mediante los servicios, donde los objetos de cliente a menudo son responsables de la ubicación del servicio.

El patrón de inserción de dependencias es una implementación determinada de inversión de Control. **Inversión de Control (IoC)** significa que los objetos no crear otros objetos que dependen para hacer su trabajo. Obtiene los objetos que necesitan desde un origen externo (por ejemplo, un archivo de configuración xml).

**Inserción de dependencias (DI)** significa que esto se realiza sin la intervención del objeto, normalmente por un componente de marco de trabajo que pasa los parámetros del constructor y establecer las propiedades.

<a id="The_Dependency_Injection_DI_Design_Pattern"></a>
### <a name="the-dependency-injection-di-design-pattern"></a>El patrón de diseño de dependencia (DI) de la inyección de código

En un nivel alto, el objetivo de inserción de dependencias es que una clase de cliente (por ejemplo, *el jugador*) necesita algo que satisface una interfaz (por ejemplo, *IClub*). No le importa cuál es el tipo concreto (por ejemplo, *WedgeClub WoodClub, IronClub,* o *PutterClub*), que otra persona para controlar que desee (por ejemplo, un buen *portadora*). La resolución de dependencias en ASP.NET MVC puede permitirle registrar la lógica de dependencia en otro lugar (por ejemplo, un contenedor o un *bolsa de tréboles*).

![Diagrama de inyección de dependencia](aspnet-mvc-4-dependency-injection/_static/image2.png "ilustración de inserción de dependencias")

*Inserción de dependencias - analogía de Golf*

Las ventajas de usar el patrón de inserción de dependencias y la inversión de Control son los siguientes:

- Reduce el acoplamiento de clases
- Aumenta la reutilización de código
- Mejora el mantenimiento del código
- Mejora de las pruebas de aplicaciones

> [!NOTE]
> Inserción de dependencias a veces se compara con el patrón de diseño Abstract Factory, pero hay una pequeña diferencia entre ambos enfoques. Inserción de dependencias tiene un marco de trabajo subyacente para resolver las dependencias mediante una llamada a los generadores y los servicios registrados.


Ahora que comprende el patrón de inserción de dependencia, aprenderá a lo largo de este laboratorio aplicarlo en ASP.NET MVC 4. Se iniciará mediante la inserción de dependencia en el **controladores** para incluir un servicio de acceso de la base de datos. A continuación, se aplicará la inserción de dependencias para el **vistas** para consumir un servicio y mostrar información. Por último, extenderá la inserción de dependencias a los filtros de ASP.NET MVC 4, inyección de un filtro de acción personalizada en la solución.

En este laboratorio práctico, aprenderá cómo:

- Integración de ASP.NET MVC 4 con Unity para la inserción de dependencias con paquetes de NuGet
- Usar inserción de dependencias dentro de un controlador ASP.NET MVC
- Usar inserción de dependencias dentro de una vista de MVC de ASP.NET
- Usar inserción de dependencias dentro de un filtro de acción de MVC de ASP.NET

> [!NOTE]
> Este laboratorio usa Unity.Mvc3 paquete de NuGet para la resolución de dependencia, pero es posible adaptar cualquier marco de inserción de dependencia para que funcione con ASP.NET MVC 4.


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

Esta práctica se compone de los ejercicios siguientes:

1. [Ejercicio 1: Insertar un controlador](#Exercise1)
2. [Ejercicio 2: Inserción de una vista](#Exercise2)
3. [Ejercicio 3: Inserción de filtros](#Exercise3)

> [!NOTE]
> Cada ejercicio está acompañado por un **final** carpeta que contiene la solución resultante debe obtener después de completar los ejercicios. Puede usar esta solución como una guía si necesita ayuda adicional para trabajar a través de los ejercicios.


Tiempo estimado para completar esta práctica: **30 minutos**.

<a id="Exercise1"></a>

<a id="Exercise_1_Injecting_a_Controller"></a>
### <a name="exercise-1-injecting-a-controller"></a>Ejercicio 1: Insertar un controlador

En este ejercicio, obtendrá información sobre cómo usar la inserción de dependencias en controladores de MVC de ASP.NET mediante la integración de Unity mediante un paquete de NuGet. Por ese motivo, se van a incluir servicios en los controladores MvcMusicStore para separar la lógica de acceso a los datos. Los servicios creación una nueva dependencia en el constructor del controlador, que se resolverá mediante la inserción de dependencias con la Ayuda de **Unity**.

Este enfoque le mostrará cómo generar menos aplicaciones acopladas, que son más flexibles y fáciles de mantener y probar. También aprenderá a integrar ASP.NET MVC con Unity.

<a id="About_StoreManager_Service"></a>
#### <a name="about-storemanager-service"></a>Acerca del servicio StoreManager

El Store de música de MVC proporciona ahora en la solución de inicio incluye un servicio que administra los datos de Store controlador denominados **StoreService**. A continuación encontrará la implementación del servicio Store. Tenga en cuenta que todos los métodos devuelven las entidades del modelo.

[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample1.cs)]

**StoreController** desde begin solución ahora consume **StoreService**. Se quitaron todas las referencias de datos **StoreController**y ahora es posible modificar el proveedor de acceso de datos actual sin cambiar cualquier método que consume **StoreService**.

A continuación, encontrará el **StoreController** implementación tiene una dependencia con **StoreService** dentro del constructor de clase.

> [!NOTE]
> Está relacionado con la dependencia que se introdujo en este ejercicio **inversión de Control** (IoC).
> 
> El **StoreController** constructor de clase recibe un **IStoreService** parámetro de tipo, lo cual es esencial para realizar llamadas a servicios desde dentro de la clase. Sin embargo, **StoreController** no implementa el constructor predeterminado (sin parámetros) que cualquier controlador debe tener para que funcione con ASP.NET MVC.
> 
> Para resolver la dependencia, el controlador tiene que va a crear una fábrica abstracta (una clase que devuelve cualquier objeto del tipo especificado).


[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample2.cs)]

> [!NOTE]
> Se producirá un error cuando la clase intenta crear el StoreController sin necesidad de enviar el objeto de servicio, ya que no hay ningún constructor sin parámetros declarado.


<a id="Ex1Task1"></a>

<a id="Task_1_-_Running_the_Application"></a>
#### <a name="task-1---running-the-application"></a>Tarea 1: ejecutar la aplicación

En esta tarea, ejecutará la aplicación de inicio, que incluye el servicio en el controlador de Store que separa el acceso a datos de la lógica de aplicación.

Al ejecutar la aplicación, recibirá una excepción, como el servicio de controlador no se pasa como un parámetro de forma predeterminada:

1. Abra el **comenzar** solución se encuentra en **Controller\Begin inyección de Source\Ex01**.

   1. Deberá descargar algunos paquetes de NuGet que faltan antes de continuar. Para ello, haga clic en el **proyecto** menú y seleccione **administrar paquetes de NuGet**.
   2. En el **administrar paquetes de NuGet** cuadro de diálogo, haga clic en **restaurar** para descargar los paquetes que falten.
   3. Por último, compile la solución haciendo **compilar** | **compilar solución**.

      > [!NOTE]
      > Una de las ventajas de usar NuGet es que no tienen que enviar todas las bibliotecas en el proyecto, lo que reduce el tamaño del proyecto. Con las herramientas avanzadas de NuGet, mediante la especificación de las versiones del paquete en el archivo Packages.config, podrá descargar todas las bibliotecas requeridas en la primera vez que ejecute el proyecto. Esto es por eso tendrá que ejecutar estos pasos después de abrir una solución existente de este laboratorio.
2. Presione **Ctrl + F5** para ejecutar la aplicación sin depuración. Obtendrá el mensaje de error &quot; **ningún constructor sin parámetros definido para este objeto**&quot;:

    ![Error al ejecutar la aplicación de ASP.NET MVC comenzar](aspnet-mvc-4-dependency-injection/_static/image3.png "Error mientras se ejecuta la aplicación de ASP.NET MVC comenzar")

    *Error al ejecutar la aplicación de ASP.NET MVC comenzar*
3. Cierre el explorador.

En los pasos siguientes funcionará en la solución de Music Store para insertar la dependencia que tiene este controlador.

<a id="Ex1Task2"></a>

<a id="Task_2_-_Including_Unity_into_MvcMusicStore_Solution"></a>
#### <a name="task-2---including-unity-into-mvcmusicstore-solution"></a>Tarea 2 - Unity incluidos en la solución MvcMusicStore

En esta tarea, se van a incluir **Unity.Mvc3** paquete NuGet para la solución.

> [!NOTE]
> Paquete Unity.Mvc3 se diseñó para ASP.NET MVC 3, pero es totalmente compatible con ASP.NET MVC 4.
> 
> Unity es un contenedor de inserción de dependencias ligero y extensible con compatibilidad opcional por ejemplo y la intercepción de tipos. Es un contenedor de uso general para su uso en cualquier tipo de aplicación. NET. Proporciona todas las características comunes de mecanismos de inyección de dependencia como: creación de objetos, la abstracción de los requisitos mediante la especificación de dependencias en tiempo de ejecución y la flexibilidad, mediante el aplazamiento de la configuración del componente al contenedor.


1. Instalar **Unity.Mvc3** paquete NuGet en el **MvcMusicStore** proyecto. Para ello, abra el **Package Manager Console** desde **vista** | **Other Windows**.
2. Ejecute el siguiente comando.

    PMC

    [!code-powershell[Main](aspnet-mvc-4-dependency-injection/samples/sample3.ps1)]

    ![Instalar el paquete de NuGet Unity.Mvc3](aspnet-mvc-4-dependency-injection/_static/image4.png "instalar el paquete de NuGet Unity.Mvc3")

    *Instalar el paquete de NuGet Unity.Mvc3*
3. Una vez el **Unity.Mvc3** está instalado el paquete, explorar los archivos y carpetas se agrega automáticamente con el fin de simplificar la configuración de Unity.

    ![Instalado el paquete Unity.Mvc3](aspnet-mvc-4-dependency-injection/_static/image5.png "paquete Unity.Mvc3 instalado")

    *Paquete Unity.Mvc3 instalado*

<a id="Ex1Task3"></a>

<a id="Task_3_-_Registering_Unity_in_Globalasaxcs_Application_Start"></a>
#### <a name="task-3---registering-unity-in-globalasaxcs-applicationstart"></a>Tarea 3: registro de Unity en la aplicación de Global.asax.cs\_iniciar

En esta tarea, actualizará la **aplicación\_iniciar** método ubicado en **Global.asax.cs** llamar el inicializador de programa previo de Unity y, a continuación, actualizar el registro de archivo de programa previo el servicio y el controlador que usará para la inserción de dependencias.

1. Ahora, se enlazará el programa previo que es el archivo que se inicializa el contenedor de Unity y resolución de dependencia. Para ello, abra **Global.asax.cs** y agregue el código resaltado siguiente dentro de la **aplicación\_iniciar** método.

    (Código de fragmento de código - *laboratorio de inyección de dependencia ASP.NET - Ex01 - inicializar Unity*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample4.cs)]
2. Abra **Bootstrapper.cs** archivo.
3. Incluir los espacios de nombres siguientes: **MvcMusicStore.Services** y **MusicStore.Controllers**.

    (Código de fragmento de código - *ASP.NET dependencia inyección laboratorio - Ex01 - programa previo agregar espacios de nombres*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample5.cs)]
4. Reemplace **BuildUnityContainer** del contenido con el siguiente código que registra el controlador de Store y el servicio Store método.

    (Código de fragmento de código - *ASP.NET dependencia inyección laboratorio - Ex01 - Store Register controlador y el servicio*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample6.cs)]

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Tarea 4: ejecución de la aplicación

En esta tarea, ejecutará la aplicación para comprobar que ahora se puede cargar después de incluir Unity.

1. Presione **F5** para ejecutar la aplicación, la aplicación debe cargar ahora sin mostrar ningún mensaje de error.

    ![Ejecutar la aplicación con la inserción de dependencias](aspnet-mvc-4-dependency-injection/_static/image6.png "ejecutando la aplicación con la inserción de dependencias")

    *Aplicación en ejecución con inserción de dependencias*
2. Vaya a **/Store**. Esto invocará **StoreController**, que ya se ha creado mediante **Unity**.

    ![MVC Music Store](aspnet-mvc-4-dependency-injection/_static/image7.png "Store música de MVC")

    *MVC Music Store*
3. Cierre el explorador.

En los ejercicios siguientes obtendrá información sobre cómo ampliar el ámbito de la inserción de dependencias para usarlo dentro de las vistas de MVC de ASP.NET y los filtros de acción.

<a id="Exercise2"></a>

<a id="Exercise_2_Injecting_a_View"></a>
### <a name="exercise-2-injecting-a-view"></a>Ejercicio 2: Inserción de una vista

En este ejercicio, obtendrá información sobre cómo usar la inserción de dependencias en una vista con las nuevas características de ASP.NET MVC 4 para la integración de Unity. Para ello, llamará a un servicio personalizado dentro de la vista de examinar Store, que mostrará un mensaje y una imagen de abajo.

A continuación, se integran el proyecto con Unity y crear una resolución de dependencia personalizadas para insertar las dependencias.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Creating_a_View_that_Consumes_a_Service"></a>
#### <a name="task-1---creating-a-view-that-consumes-a-service"></a>Tarea 1: crear una vista que consume un servicio

En esta tarea, creará una vista que realiza una llamada de servicio para generar una nueva dependencia. El servicio se compone de un servicio de mensajería simple incluido en esta solución.

1. Abra el **comenzar** solución se encuentra en la **inyección de Source\Ex02 View\Begin** carpeta. En caso contrario, es posible que siga usando la **final** solución obtenido completando el ejercicio anterior.

   1. Si abrió proporcionado **comenzar** solución, deberá descargar algunos paquetes de NuGet que faltan antes de continuar. Para ello, haga clic en el **proyecto** menú y seleccione **administrar paquetes de NuGet**.
   2. En el **administrar paquetes de NuGet** cuadro de diálogo, haga clic en **restaurar** para descargar los paquetes que falten.
   3. Por último, compile la solución haciendo **compilar** | **compilar solución**.

      > [!NOTE]
      > Una de las ventajas de usar NuGet es que no tienen que enviar todas las bibliotecas en el proyecto, lo que reduce el tamaño del proyecto. Con las herramientas avanzadas de NuGet, mediante la especificación de las versiones del paquete en el archivo Packages.config, podrá descargar todas las bibliotecas requeridas en la primera vez que ejecute el proyecto. Esto es por eso tendrá que ejecutar estos pasos después de abrir una solución existente de este laboratorio.
      > 
      > Para obtener más información, consulte este artículo: [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages ](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).
2. Incluyen la **MessageService.cs** y el **IMessageService.cs** clases se encuentran en el **origen \Assets** carpeta en **/servicios**. Para ello, haga clic en **servicios** carpeta y seleccione **Agregar elemento existente**. Vaya a la ubicación de los archivos y los incluye.

    ![Agregar servicio de mensajes y la interfaz de servicio](aspnet-mvc-4-dependency-injection/_static/image8.png "Agregar servicio de mensajes y la interfaz de servicio")

    *Agregar servicio de mensajes y la interfaz de servicio*

    > [!NOTE]
    > El **IMessageService** interfaz define dos propiedades implementadas por el **mensaje** clase. Estas propiedades -**mensaje** y **ImageUrl**-almacenar el mensaje y la dirección URL de la imagen que se muestre.
3. Cree la carpeta **/páginas** en el proyecto de la carpeta raíz y, a continuación, agregue la clase existente **MyBasePage.cs** desde **Source\Assets**. La página base de que se heredarán tiene la siguiente estructura.

    ![Carpeta páginas](aspnet-mvc-4-dependency-injection/_static/image9.png "carpeta páginas")

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample7.cs)]
4. Abra **Browse.cshtml** desde la vista **/Views/Store** carpeta y hacer que heredan de **MyBasePage.cs**.

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample8.cshtml)]
5. En el **examinar** ver, agregar una llamada a **mensaje** para mostrar una imagen y un mensaje recuperado por el servicio.
   (C#)

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample9.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Including_a_Custom_Dependency_Resolver_and_a_Custom_View_Page_Activator"></a>
#### <a name="task-2---including-a-custom-dependency-resolver-and-a-custom-view-page-activator"></a>Tarea 2: como una resolución de dependencia personalizada y un activador de página de vista personalizada

En la tarea anterior, se inserta una nueva dependencia dentro de una vista para realizar una llamada de servicio dentro de él. Ahora, se resolverá esa dependencia implementando las interfaces de inserción de dependencias de MVC de ASP.NET **IViewPageActivator** y **IDependencyResolver**. Se van a incluir en la solución de una implementación de **IDependencyResolver** que se ocupará la recuperación del servicio mediante el uso de Unity. A continuación, se van a incluir otra implementación personalizada de **IViewPageActivator** interfaz destinadas a resolver la creación de las vistas.

> [!NOTE]
> Desde ASP.NET MVC 3, la implementación para la inyección de dependencia tenía simplificado las interfaces para registrar los servicios. **IDependencyResolver** y **IViewPageActivator** forman parte de las características de ASP.NET MVC 3 para la inserción de dependencias.
> 
> **-IDependencyResolver** interfaz reemplaza el IMvcServiceLocator anterior. Los implementadores de IDependencyResolver deben devolver una instancia del servicio o una colección de servicios.
> 
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample10.cs)]
> 
> **-IViewPageActivator** interfaz proporciona un mayor control sobre cómo se crean instancias de las páginas de vista a través de la inserción de dependencias. Las clases que implementan **IViewPageActivator** interfaz puede crear instancias de vista utilizando la información de contexto.
> 
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample11.cs)]


1. Crear el /**generadores** carpeta en la carpeta raíz del proyecto.
2. Incluir **CustomViewPageActivator.cs** a la solución de **/orígenes/recursos/** a **generadores** carpeta. Para ello, haga clic en el **/Factories** carpeta, seleccione **Add | Elemento existente** y, a continuación, seleccione **CustomViewPageActivator.cs**. Esta clase implementa la **IViewPageActivator** interfaz para albergar el contenedor de Unity.

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample12.cs)]

    > [!NOTE]
    > **CustomViewPageActivator** es responsable de administrar la creación de una vista mediante el uso de un contenedor de Unity.
3. Incluir **UnityDependencyResolver.cs** de archivos de **/Sources/activos** a **/Factories** carpeta. Para ello, haga clic en el **/Factories** carpeta, seleccione **Add | Elemento existente** y, a continuación, seleccione **UnityDependencyResolver.cs** archivo.

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample13.cs)]

    > [!NOTE]
    > **UnityDependencyResolver** clase es un DependencyResolver personalizado para Unity. Cuando un servicio no se encuentra dentro del contenedor de Unity, la resolución base es invocated.

En la siguiente tarea ambas implementaciones se registrará para permitir que el modelo se conoce la ubicación de los servicios y las vistas.

<a id="Ex2Task3"></a>

<a id="Task_3_-_Registering_for_Dependency_Injection_within_Unity_container"></a>
#### <a name="task-3---registering-for-dependency-injection-within-unity-container"></a>Tarea 3: registro de inserción de dependencias dentro del contenedor de Unity

En esta tarea, se colocará todo lo anterior para realizar la inserción de dependencias funcione.

Hasta ahora, la solución tiene los siguientes elementos:

- Un **examinar** vista que hereda de **MyBaseClass** y consume **mensaje**.
- Una clase intermedia -**MyBaseClass**-que se declara de la interfaz de servicio de inserción de dependencias.
- Un servicio - **mensaje** - y su interfaz **IMessageService**.
- Una resolución de dependencia personalizadas para Unity: **UnityDependencyResolver** -que se ocupa de recuperación de servicio.
- Un activador de página de vista - **CustomViewPageActivator** -que crea la página.

Insertar **examinar** vista, ahora se registrará la resolución de dependencia personalizadas en el contenedor de Unity.

1. Abra **Bootstrapper.cs** archivo.
2. Registrar una instancia de **mensaje** en el contenedor de Unity para inicializar el servicio:

    (Código de fragmento de código - *servicio de mensaje de registro de laboratorio - Ex02 - inserción de dependencias ASP.NET*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample14.cs)]
3. Agregue una referencia a **MvcMusicStore.Factories** espacio de nombres.

    (Código de fragmento de código - *ASP.NET dependencia inyección laboratorio - Ex02 - generadores Namespace*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample15.cs)]
4. Registrar **CustomViewPageActivator** como un activador de página de vista en el contenedor de Unity:

    (Código de fragmento de código - *ASP.NET dependencia inyección laboratorio - Ex02 - Register CustomViewPageActivator*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample16.cs)]
5. Reemplace la resolución de dependencia predeterminada ASP.NET MVC 4 con una instancia de **UnityDependencyResolver**. Para ello, reemplace **Initialise** método contenido con el código siguiente:

    (Código de fragmento de código - *resolución de dependencia de actualización ASP.NET dependencia inyección laboratorio - Ex02 -*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample17.cs)]

    > [!NOTE]
    > ASP.NET MVC proporciona una clase de resolución de dependencia predeterminada. Para trabajar con las resoluciones de dependencia personalizadas que hemos creado por unity, esta resolución debe reemplazarse.

<a id="Ex2Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Tarea 4: ejecución de la aplicación

En esta tarea, ejecutará la aplicación para comprobar que el Explorador de Store consume el servicio y muestra la imagen y el mensaje recuperado:

1. Presione **F5** para ejecutar la aplicación.
2. Haga clic en **Rock** dentro del menú de géneros y vea cómo el **mensaje** insertado a la vista y se cargan el mensaje de bienvenida y la imagen. En este ejemplo, estamos entrando a &quot; **Rock**&quot;:

    ![MVC Music Store - inserción de vista](aspnet-mvc-4-dependency-injection/_static/image10.png "MVC Music Store - inserción de vista")

    *MVC Music Store - inserción de vista*
3. Cierre el explorador.

<a id="Exercise3"></a>

<a id="Exercise_3_Injecting_Action_Filters"></a>
### <a name="exercise-3-injecting-action-filters"></a>Ejercicio 3: Inserción de los filtros de acción

En la práctica anterior **filtros de acción personalizados** ha trabajado con la personalización de los filtros y por inyección de código. En este ejercicio, obtendrá información sobre cómo insertar los filtros con inserción de dependencias mediante el contenedor de Unity. Para ello, se agregará a la solución de Music Store un filtro de acción personalizada que se trazará la actividad del sitio.

<a id="Ex3Task1"></a>

<a id="Task_1_-_Including_the_Tracking_Filter_in_the_Solution"></a>
#### <a name="task-1---including-the-tracking-filter-in-the-solution"></a>Tarea 1: incluido el filtro de seguimiento de la solución

En esta tarea, se van a incluir en el Store música un filtro de acción personalizada para los eventos de seguimiento. Como filtro de acción personalizado ya se tratan los conceptos en la práctica anterior &quot;filtros de acción personalizado&quot;, que solo incluya la clase de filtro de la carpeta recursos de este laboratorio y, a continuación, crear un proveedor de filtros para Unity:

1. Abra el **comenzar** solución se encuentra en la **Source\Ex03 - Insertar acción Filter\Begin** carpeta. En caso contrario, es posible que siga usando la **final** solución obtenido completando el ejercicio anterior.

   1. Si abrió proporcionado **comenzar** solución, deberá descargar algunos paquetes de NuGet que faltan antes de continuar. Para ello, haga clic en el **proyecto** menú y seleccione **administrar paquetes de NuGet**.
   2. En el **administrar paquetes de NuGet** cuadro de diálogo, haga clic en **restaurar** para descargar los paquetes que falten.
   3. Por último, compile la solución haciendo **compilar** | **compilar solución**.

      > [!NOTE]
      > Una de las ventajas de usar NuGet es que no tienen que enviar todas las bibliotecas en el proyecto, lo que reduce el tamaño del proyecto. Con las herramientas avanzadas de NuGet, mediante la especificación de las versiones del paquete en el archivo Packages.config, podrá descargar todas las bibliotecas requeridas en la primera vez que ejecute el proyecto. Esto es por eso tendrá que ejecutar estos pasos después de abrir una solución existente de este laboratorio.
      > 
      > Para obtener más información, consulte este artículo: [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages ](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).
2. Incluir **TraceActionFilter.cs** de archivos de **/Sources/activos** a **/filtra** carpeta.

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample18.cs)]

    > [!NOTE]
    > Este filtro de acción personalizada realiza el seguimiento en ASP.NET. Puede comprobar &quot;local de ASP.NET MVC 4 y los filtros de acción dinámica&quot; laboratorio para obtener más referencias.
3. Agregue la clase vacía **FilterProvider.cs** al proyecto en la carpeta   **/filtros.**
4. Agregar el **System.Web.Mvc** y **Microsoft.Practices.Unity** espacios de nombres en **FilterProvider.cs**.

    (Código de fragmento de código - *ASP.NET dependencia inyección laboratorio - Ex03 - Filtro proveedor agregar espacios de nombres*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample19.cs)]
5. Hacer que herede de la clase **IFilterProvider** interfaz.

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample20.cs)]
6. Agregar un **IUnityContainer** propiedad en el **FilterProvider** clase y, a continuación, cree un constructor de clase para asignar el contenedor.

    (Código de fragmento de código - *ASP.NET dependencia inyección laboratorio - Ex03 - Filtro proveedor Constructor*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample21.cs)]

    > [!NOTE]
    > El constructor de clase de proveedor de filtro no está creando un **nuevo** dentro de objeto. El contenedor se pasa como parámetro y se soluciona la dependencia de Unity.
7. En el **FilterProvider** class, implemente el método **GetFilters** desde **IFilterProvider** interfaz.

    (Código de fragmento de código - *ASP.NET dependencia inyección laboratorio - Ex03 - Filtro proveedor GetFilters*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample22.cs)]

<a id="Ex3Task2"></a>

<a id="Task_2_-_Registering_and_Enabling_the_Filter"></a>
#### <a name="task-2---registering-and-enabling-the-filter"></a>Tarea 2: registrar y habilitar el filtro

En esta tarea, se habilita el seguimiento del sitio. Para ello, se registrará el filtro en **Bootstrapper.cs BuildUnityContainer** método para iniciar el seguimiento:

1. Abra **Web.config** ubicado en la raíz del proyecto y habilitar el seguimiento de traza en el grupo de System.Web.

    [!code-xml[Main](aspnet-mvc-4-dependency-injection/samples/sample23.xml)]
2. Abra **Bootstrapper.cs** en la raíz del proyecto.
3. Agregue una referencia a la **MvcMusicStore.Filters** espacio de nombres.

    (Código de fragmento de código - *ASP.NET dependencia inyección laboratorio - Ex03 - programa previo agregar espacios de nombres*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample24.cs)]
4. Seleccione el **BuildUnityContainer** método y registrar el filtro en el contenedor de Unity. Tendrá que registrar el proveedor de filtros, así como el filtro de acción.

    (Código de fragmento de código - *ASP.NET dependencia inyección laboratorio - Ex03 - Register FilterProvider y ActionFilter*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample25.cs)]

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Tarea 3: ejecutar la aplicación

En esta tarea, ejecutará la aplicación y que el filtro de acción personalizada seguimiento la actividad de prueba:

1. Presione **F5** para ejecutar la aplicación.
2. Haga clic en **Rock** en el menú de géneros. Puede examinar varios géneros si desea.

    ![Music Store](aspnet-mvc-4-dependency-injection/_static/image11.png "Music Store")

    *Tienda de música*
3. Vaya a **/Trace.axd** para ver el seguimiento de la aplicación de página y, a continuación, haga clic en **ver detalles**.

    ![Registro de seguimiento de la aplicación](aspnet-mvc-4-dependency-injection/_static/image12.png "registro de seguimiento de la aplicación")

    *Registro de seguimiento de la aplicación*

    ![Seguimiento de la aplicación - detalles de la solicitud](aspnet-mvc-4-dependency-injection/_static/image13.png "aplicación Trace - detalles de la solicitud")

    *Seguimiento de la aplicación - detalles de la solicitud*
4. Cierre el explorador.

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Resumen

Al completar este laboratorio práctico ha aprendido a usar la inserción de dependencias en ASP.NET MVC 4 mediante la integración de Unity mediante un paquete de NuGet. Para lograrlo, ha usado la inserción de dependencias dentro de los controladores, vistas y los filtros de acción.

Se tratan los conceptos siguientes:

- Características de inserción de dependencias de ASP.NET MVC 4
- Integración de Unity mediante el paquete de NuGet Unity.Mvc3
- Inserción de dependencias en controladores
- Inserción de dependencias en vistas
- Inserción de dependencias de los filtros de acción

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Apéndice A: instalación de Visual Studio Express 2012 para Web

Puede instalar **Microsoft Visual Studio Express 2012 para Web** u otro &quot;Express&quot; versión utilizando el **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**. Las instrucciones siguientes le guían a través de los pasos necesarios para instalar *Visual studio Express 2012 para Web* mediante *Microsoft Web Platform Installer*.

1. Vaya a [ [ https://go.microsoft.com/? linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). Como alternativa, si ya ha instalado el instalador de plataforma Web, puede abrirla y busque el producto &quot; <em>Visual Studio Express 2012 for Web con SDK de Windows Azure</em>&quot;.
2. Haga clic en **instalar ahora**. Si no tienes **instalador de plataforma Web** se le redirigirá para descargarlo e instalarlo en primer lugar.
3. Una vez **instalador de plataforma Web** está abierto, haga clic en **instalar** para iniciar el programa de instalación.

    ![Instale Visual Studio Express](aspnet-mvc-4-dependency-injection/_static/image14.png "instale Visual Studio Express")

    *Instale Visual Studio Express*
4. Leer todos los productos las licencias y los términos y haga clic en **acepto** para continuar.

    ![Acepte los términos de licencia](aspnet-mvc-4-dependency-injection/_static/image15.png)

    *Acepte los términos de licencia*
5. Espere hasta que finalice el proceso de descarga e instalación.

    ![Progreso de la instalación](aspnet-mvc-4-dependency-injection/_static/image16.png)

    *Progreso de la instalación*
6. Cuando se complete la instalación, haga clic en **finalizar**.

    ![Instalación completada](aspnet-mvc-4-dependency-injection/_static/image17.png)

    *Instalación completada*
7. Haga clic en **Exit** para cerrar el instalador de plataforma Web.
8. Para abrir Visual Studio Express para Web, vaya a la **iniciar** pantalla y comienza a escribir &quot; **VS Express**&quot;, a continuación, haga clic en el **VS Express para Web** icono.

    ![VS Express para el icono de Web](aspnet-mvc-4-dependency-injection/_static/image18.png)

    *VS Express para el icono de Web*

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a>Apéndice B: uso de fragmentos de código

Con fragmentos de código, tiene todo el código que necesita a su alcance. El documento de laboratorio le dirá exactamente cuándo se pueden utilizar, como se muestra en la ilustración siguiente.

![Uso de fragmentos de código de Visual Studio para insertar código en el proyecto](aspnet-mvc-4-dependency-injection/_static/image19.png "fragmentos de código en Visual Studio para insertar código en el proyecto")

*Uso de fragmentos de código de Visual Studio para insertar código en el proyecto*

***Para agregar un fragmento de código mediante el teclado (solo C#)***

1. Coloque el cursor donde desea insertar el código.
2. Comience a escribir el nombre del fragmento de código (sin espacios ni guiones).
3. Observe cómo IntelliSense muestra los nombres de fragmentos de código coincidentes.
4. Seleccione el fragmento de código correcto (o siga escribiendo hasta que se selecciona el nombre del fragmento de código completo).
5. Presione la tecla Tab dos veces para insertar el fragmento de código en la ubicación del cursor.

![Comience a escribir el nombre del fragmento](aspnet-mvc-4-dependency-injection/_static/image20.png "comience a escribir el nombre del fragmento de código")

*Comience a escribir el nombre del fragmento de código*

![Presione la tecla Tab para seleccionar el fragmento de código resaltada](aspnet-mvc-4-dependency-injection/_static/image21.png "presione Tab para seleccionar el fragmento de código resaltada")

*Presione la tecla Tab para seleccionar el fragmento de código resaltada*

![Vuelva a presionar Tab y el fragmento de código se expandirán](aspnet-mvc-4-dependency-injection/_static/image22.png "vuelva a presionar Tab y el fragmento de código se expandirán")

*Vuelva a presionar Tab y el fragmento de código se expandirán*

***Para agregar un fragmento de código con el mouse (C#, en Visual Basic y XML)*** 1. Haga doble clic en el desea insertar el fragmento de código.

1. Seleccione **Insertar fragmento de código** seguido **Mis fragmentos de código**.
2. Seleccione el fragmento de código relevante en la lista, haciendo clic en él.

![Con el botón secundario donde desea insertar el fragmento de código y seleccione Insertar fragmento de código](aspnet-mvc-4-dependency-injection/_static/image23.png "contextual donde desea insertar el fragmento de código y seleccione Insertar fragmento de código")

*Haga doble clic donde desea insertar el fragmento de código y seleccione Insertar fragmento de código*

![Elegir el fragmento de código relevante en la lista, hacer clic en ella](aspnet-mvc-4-dependency-injection/_static/image24.png "elegir el fragmento de código relevante en la lista, hacer clic en ella")

*Elegir el fragmento de código relevante en la lista, hacer clic en ella*
