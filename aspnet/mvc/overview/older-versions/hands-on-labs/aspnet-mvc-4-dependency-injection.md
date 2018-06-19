---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
title: Inserción de dependencias de MVC de ASP.NET 4 | Documentos de Microsoft
author: rick-anderson
description: 'Nota: Este laboratorio práctico sobre supone que tiene conocimientos básicos de los filtros ASP.NET MVC y ASP.NET MVC 4. Si no ha utilizado filtros de ASP.NET MVC 4 antes de, rec...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 84c7baca-1c54-4c44-8f52-4282122d6acb
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: e6c24d03039f0e6005948a73348589627c9df2df
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30877663"
---
# <a name="aspnet-mvc-4-dependency-injection"></a>Inserción de dependencias de MVC de ASP.NET 4

Por [Web colonias equipo](https://twitter.com/webcamps)

[Descargar el Kit de aprendizaje de colonias de Web](https://aka.ms/webcamps-training-kit)

Este laboratorio práctico se da por supuesto que tiene conocimientos básicos de **ASP.NET MVC** y **filtros de ASP.NET MVC 4**. Si no ha usado **filtros de ASP.NET MVC 4** antes, le recomendamos que repase **filtros de acción de ASP.NET MVC personalizado** laboratorio práctico.

> [!NOTE]
> Todo el código de ejemplo y fragmentos de código se incluyen en el Kit de aprendizaje de Web colonias, disponible desde en [versiones de Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit). Está disponible en el proyecto específico para este laboratorio [inyección de dependencia de ASP.NET MVC 4](https://github.com/Microsoft-Web/HOL-MVC4DependencyInjection).

En **la programación orientada a objetos de objeto** paradigma, objetos trabajan juntos en un modelo de colaboración donde hay colaboradores y los consumidores. Naturalmente, este modelo de comunicación genera las dependencias entre los objetos y componentes, pase a ser difíciles de administrar cuando aumenta la complejidad.

![Clase dependencias y la complejidad del modelo](aspnet-mvc-4-dependency-injection/_static/image1.png "clase dependencias y la complejidad del modelo")

*Dependencias de la clase y la complejidad de modelo*

Probablemente habrá oído hablar sobre la **modelo de generador** y la separación entre la interfaz y la implementación mediante los servicios, donde los objetos de cliente a menudo son responsables de la ubicación del servicio.

El modelo de inserción de dependencias es una implementación concreta de inversión de Control. **Inversión de Control (IoC)** significa que los objetos no crear otros objetos en el que se basan para hacer su trabajo. En su lugar, obtienen los objetos que necesitan de un origen externo (por ejemplo, un archivo de configuración xml).

**Inyección de dependencia (DI)** significa que esto se realiza sin la intervención del objeto, normalmente por un componente de marco de trabajo que pasa los parámetros de constructor y establecer las propiedades.

<a id="The_Dependency_Injection_DI_Design_Pattern"></a>
### <a name="the-dependency-injection-di-design-pattern"></a>El modelo de diseño de dependencia (DI) por inyección de código

En un nivel alto, el objetivo de inserción de dependencias es que una clase de cliente (por ejemplo, *el golfista*) necesita algo que satisface una interfaz (por ejemplo, *IClub*). No le importa qué es un tipo concreto (p. ej. *WoodClub, IronClub, WedgeClub* o *PutterClub*), que alguien pueda controlar que desee (por ejemplo, un buen *portadora*). La resolución de dependencia en ASP.NET MVC le permiten registrar su lógica de dependencia en otro lugar (por ejemplo, un contenedor o un *bolsa de tréboles*).

![Diagrama de inyección de dependencia](aspnet-mvc-4-dependency-injection/_static/image2.png "ilustración de inyección de dependencia")

*Inserción de dependencias - analogía de Golf*

Las ventajas de utilizar el modelo de inserción de dependencias y la inversión de Control son los siguientes:

- Reduce el acoplamiento de clase
- Aumenta la reutilización de código
- Mejora el mantenimiento del código
- Mejora de pruebas de la aplicación

> [!NOTE]
> Inyección de dependencia a veces se compara con el patrón de diseño Factory abstracta, pero hay una ligera diferencia entre ambos enfoques. DI tiene un marco de trabajo detrás para resolver las dependencias mediante una llamada a los generadores y los servicios registrados.


Ahora que comprende el patrón de inyección de dependencia, aprenderá a lo largo de este laboratorio que se aplicará en ASP.NET MVC 4. Se iniciará mediante la inserción de dependencia en el **controladores** para incluir un servicio de acceso de la base de datos. A continuación, se aplicará la inyección de dependencia para el **vistas** para consumir un servicio y mostrar información. Por último, se ampliará la DI a los filtros de ASP.NET MVC 4, inserte un filtro de acción personalizada en la solución.

En este laboratorio práctico, aprenderá cómo:

- Integración de ASP.NET MVC 4 con Unity para la inyección de dependencia con los paquetes de NuGet
- Usar la inserción de dependencias dentro de un controlador de MVC de ASP.NET
- Usar la inserción de dependencias dentro de una vista de MVC de ASP.NET
- Usar la inserción de dependencias dentro de un filtro de acción de MVC de ASP.NET

> [!NOTE]
> Esta práctica es usar Unity.Mvc3 paquete de NuGet para la resolución de dependencia, pero es posible adaptar cualquier marco de inyección de dependencia para que funcione con ASP.NET MVC 4.


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

Si no está familiarizado con los fragmentos de código de Visual Studio y desea obtener información sobre cómo utilizarlas, puede consultar el apéndice de este documento &quot; [Apéndice B: Using Code Snippets](#AppendixB)&quot;.

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Ejercicios

Este laboratorio práctico se compone de los ejercicios siguientes:

1. [Ejercicio 1: Insertar un controlador](#Exercise1)
2. [Ejercicio 2: Inserte una vista](#Exercise2)
3. [Ejercicio 3: Insertar filtros](#Exercise3)

> [!NOTE]
> Cada ejercicio está acompañado por un **final** carpeta que contiene la solución resultante debería obtener después de completar los ejercicios. Puede usar esta solución como una guía si necesita ayuda adicional para trabajar a través de los ejercicios.


Tiempo estimado para completar esta práctica: **30 minutos**.

<a id="Exercise1"></a>

<a id="Exercise_1_Injecting_a_Controller"></a>
### <a name="exercise-1-injecting-a-controller"></a>Ejercicio 1: Insertar un controlador

En este ejercicio, obtendrá información sobre cómo usar la inserción de dependencias en los controladores de MVC de ASP.NET mediante la integración de Unity mediante un paquete de NuGet. Por esta razón, se van a incluir servicios en los controladores de MvcMusicStore para separar la lógica desde el acceso a datos. Los servicios creará una nueva dependencia en el constructor del controlador, que se resolverá mediante la inserción de dependencias con la Ayuda de **Unity**.

Este enfoque le mostrará cómo generar menos aplicaciones acopladas, que son más flexibles y fáciles de mantener y probar. También obtendrá información sobre cómo integrar ASP.NET MVC con Unity.

<a id="About_StoreManager_Service"></a>
#### <a name="about-storemanager-service"></a>Acerca del servicio de StoreManager

La tienda de música de MVC proporciona ahora en la solución de inicio incluye un servicio que administra los datos de almacén controlador denominados **StoreService**. A continuación encontrará la implementación del servicio de almacén. Tenga en cuenta que todos los métodos devuelven las entidades del modelo.

[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample1.cs)]

**StoreController** desde el inicio solución consume ahora **StoreService**. Se quitaron todas las referencias de datos **StoreController**y ahora es posible modificar el proveedor de acceso de datos actual sin cambiar cualquier método que consume **StoreService**.

Encontrará a continuación que el **StoreController** implementación tiene una dependencia con **StoreService** dentro del constructor de clase.

> [!NOTE]
> Está relacionado con la dependencia que se introdujo en este ejercicio **inversión de Control** (IoC).
> 
> El **StoreController** constructor de clase recibe un **IStoreService** parámetro de tipo, que es esencial para realizar llamadas de servicio desde dentro de la clase. Sin embargo, **StoreController** no implementa el constructor predeterminado (sin ningún parámetro) que debe tener un controlador para que funcione con ASP.NET MVC.
> 
> Para resolver la dependencia, el controlador debe crearse mediante un generador abstracto (una clase que devuelve cualquier objeto del tipo especificado).


[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample2.cs)]

> [!NOTE]
> Obtendrá un error cuando la clase intenta crear la StoreController sin necesidad de enviar el objeto de servicio, porque no hay ningún constructor sin parámetros declarado.


<a id="Ex1Task1"></a>

<a id="Task_1_-_Running_the_Application"></a>
#### <a name="task-1---running-the-application"></a>Tarea 1: ejecutar la aplicación

En esta tarea, se ejecutará la aplicación de inicio, que incluye el servicio en el controlador de almacén que separa el acceso a los datos de la lógica de aplicación.

Cuando se ejecuta la aplicación, recibirá una excepción, como el servicio del controlador no se pasa como un parámetro de forma predeterminada:

1. Abra la **comenzar** soluciones se encuentran en **Controller\Begin insertar Source\Ex01**.

   1. Deberá descargar algunos paquetes de NuGet que faltan antes de continuar. Para ello, haga clic en el **proyecto** menú y seleccione **administrar paquetes de NuGet**.
   2. En el **administrar paquetes de NuGet** cuadro de diálogo, haga clic en **restaurar** para descargar los paquetes que falten.
   3. Por último, compile la solución haciendo clic en **generar** | **generar solución**.

      > [!NOTE]
      > Una de las ventajas del uso de NuGet es que no tiene que enviar todas las bibliotecas en el proyecto, lo que reduce el tamaño del proyecto. Con NuGet Power Tools, mediante la especificación de las versiones del paquete en el archivo Packages.config, podrá descargar la primera vez que ejecute el proyecto de todas las bibliotecas necesarias. Este es el motivo por el que se deben ejecutar estos pasos después de abrir una solución existente de este laboratorio.
2. Presione **Ctrl + F5** para ejecutar la aplicación sin depuración. Se le devolverá el mensaje de error &quot; **ningún constructor sin parámetros definido para este objeto**&quot;:

    ![Error al ejecutar la aplicación de ASP.NET MVC comenzar](aspnet-mvc-4-dependency-injection/_static/image3.png "Error al ejecutar la aplicación de ASP.NET MVC comenzar")

    *Error al ejecutar la aplicación de ASP.NET MVC comenzar*
3. Cierre el explorador.

En los pasos siguientes funcionará en la solución de tienda de música para insertar la dependencia de que este controlador es necesario.

<a id="Ex1Task2"></a>

<a id="Task_2_-_Including_Unity_into_MvcMusicStore_Solution"></a>
#### <a name="task-2---including-unity-into-mvcmusicstore-solution"></a>Tarea 2 - Unity incluidos en la solución MvcMusicStore

En esta tarea, se incluirá la **Unity.Mvc3** paquete NuGet para la solución.

> [!NOTE]
> Paquete de Unity.Mvc3 se diseñó para ASP.NET MVC 3, pero es totalmente compatible con ASP.NET MVC 4.
> 
> Unity es un contenedor de inyección de dependencia ligeras y extensibles con compatibilidad opcional para la instancia y escriba interceptación. Es un contenedor de uso general para su uso en cualquier tipo de aplicación. NET. Proporciona todas las características comunes de mecanismos de inyección de dependencia incluidos: creación de objetos, la abstracción de requisitos mediante la especificación de dependencias en tiempo de ejecución y la flexibilidad, ya que aplazan la configuración del componente para el contenedor.


1. Instalar **Unity.Mvc3** paquete de NuGet en la **MvcMusicStore** proyecto. Para ello, abra el **Package Manager Console** de **vista** | **otras ventanas**.
2. Ejecute el siguiente comando.

    PMC

    [!code-powershell[Main](aspnet-mvc-4-dependency-injection/samples/sample3.ps1)]

    ![Instalando paquete NuGet de Unity.Mvc3](aspnet-mvc-4-dependency-injection/_static/image4.png "Instalar paquete de NuGet Unity.Mvc3")

    *Instalando paquete NuGet de Unity.Mvc3*
3. Una vez el **Unity.Mvc3** está instalado el paquete, explorar los archivos y carpetas que agrega automáticamente con el fin de simplificar la configuración de Unity.

    ![Instalado el paquete de Unity.Mvc3](aspnet-mvc-4-dependency-injection/_static/image5.png "paquete Unity.Mvc3 instalado")

    *Paquete de Unity.Mvc3 instalado*

<a id="Ex1Task3"></a>

<a id="Task_3_-_Registering_Unity_in_Globalasaxcs_Application_Start"></a>
#### <a name="task-3---registering-unity-in-globalasaxcs-applicationstart"></a>Tarea 3: registro de Unity en aplicación Global.asax.cs\_iniciar

En esta tarea, actualizará la **aplicación\_iniciar** método ubicado en **Global.asax.cs** para llamar el inicializador de Unity del programa previo y, a continuación, actualizar el registro de archivo de programa previo el servicio y el controlador que se va a usar para la inyección de dependencia.

1. Ahora, enlazará el programa previo que es el archivo que inicializa el contenedor de Unity y resolución de dependencia. Para ello, abra **Global.asax.cs** y agregue el código que aparece resaltado en el **aplicación\_iniciar** método.

    (Código de fragmento de código: *laboratorio de inyección de dependencia ASP.NET - Ex01 - inicializar Unity*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample4.cs)]
2. Abra **Bootstrapper.cs** archivo.
3. Incluya los espacios de nombres siguientes: **MvcMusicStore.Services** y **MusicStore.Controllers**.

    (Código de fragmento de código: *agregar espacios de nombres ASP.NET dependencia inyección laboratorio - Ex01 - arranque*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample5.cs)]
4. Reemplace **BuildUnityContainer** método del contenido con el código siguiente a la que se registra el controlador de almacén y el servicio de almacenamiento.

    (Código de fragmento de código: *servicio y controlador de almacén de registro ASP.NET dependencia inyección laboratorio - Ex01 -*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample6.cs)]

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Tarea 4: ejecutar la aplicación

En esta tarea, se ejecutará la aplicación para comprobar que ahora se puede cargar después de incluir Unity.

1. Presione **F5** para ejecutar la aplicación, la aplicación debe cargar ahora sin mostrar ningún mensaje de error.

    ![Ejecutar la aplicación con la inserción de dependencias](aspnet-mvc-4-dependency-injection/_static/image6.png "ejecutando la aplicación con la inserción de dependencias")

    *Aplicación en ejecución con la inserción de dependencias*
2. Vaya a **/almacén**. Esto invocará **StoreController**, que se ha creado con **Unity**.

    ![Tienda de música de MVC](aspnet-mvc-4-dependency-injection/_static/image7.png "tienda de música de MVC")

    *Tienda de música de MVC*
3. Cierre el explorador.

En los siguientes ejercicios obtendrá información sobre cómo ampliar el ámbito de inyección de dependencia para usarlo dentro de las vistas de MVC de ASP.NET y los filtros de acción.

<a id="Exercise2"></a>

<a id="Exercise_2_Injecting_a_View"></a>
### <a name="exercise-2-injecting-a-view"></a>Ejercicio 2: Inserte una vista

En este ejercicio, obtendrá información sobre cómo usar la inserción de dependencias de una vista con las nuevas características de ASP.NET MVC 4 para la integración de Unity. Para ello, se llamará a un servicio personalizado dentro de la vista de examinar de almacén, que mostrará un mensaje y una imagen que aparece a continuación.

A continuación, se integran el proyecto con Unity y crear una resolución de dependencia personalizado para insertar las dependencias.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Creating_a_View_that_Consumes_a_Service"></a>
#### <a name="task-1---creating-a-view-that-consumes-a-service"></a>Tarea 1: crear una vista que consume un servicio

En esta tarea, creará una vista que realiza una llamada de servicio para generar una nueva dependencia. El servicio está compuesto de un servicio de mensajería simple incluido en esta solución.

1. Abra el **comenzar** soluciones se encuentran en la **View\Begin inyectar Source\Ex02** carpeta. En caso contrario, puede seguir usando el **final** solución obtenido siguiendo el ejercicio anterior.

   1. Si abrió proporcionado **comenzar** solución, deberá descargar algunos paquetes de NuGet que faltan antes de continuar. Para ello, haga clic en el **proyecto** menú y seleccione **administrar paquetes de NuGet**.
   2. En el **administrar paquetes de NuGet** cuadro de diálogo, haga clic en **restaurar** para descargar los paquetes que falten.
   3. Por último, compile la solución haciendo clic en **generar** | **generar solución**.

      > [!NOTE]
      > Una de las ventajas del uso de NuGet es que no tiene que enviar todas las bibliotecas en el proyecto, lo que reduce el tamaño del proyecto. Con NuGet Power Tools, mediante la especificación de las versiones del paquete en el archivo Packages.config, podrá descargar la primera vez que ejecute el proyecto de todas las bibliotecas necesarias. Este es el motivo por el que se deben ejecutar estos pasos después de abrir una solución existente de este laboratorio.
      > 
      > Para obtener más información, consulte este artículo: [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages ](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).
2. Incluir el **MessageService.cs** y la **IMessageService.cs** clases se encuentran en el **origen \Assets** carpeta en **/servicios**. Para ello, haga clic en **servicios** carpeta y seleccione **Agregar elemento existente**. Vaya a la ubicación de los archivos y los incluye.

    ![Agregar servicio de mensajes y la interfaz de servicio](aspnet-mvc-4-dependency-injection/_static/image8.png "Agregar servicio de mensajes y la interfaz de servicio")

    *Agregar servicio de mensajes y la interfaz de servicio*

    > [!NOTE]
    > El **IMessageService** interfaz define dos propiedades implementadas por el **MessageService** clase. Estas propiedades -**mensaje** y **ImageUrl**-almacenar el mensaje y la dirección URL de la imagen que se muestre.
3. Cree la carpeta **/páginas** en el proyecto de la carpeta raíz y, a continuación, agregue la clase existente **MyBasePage.cs** de **Source\Assets**. La página base de que heredará tiene la siguiente estructura.

    ![Carpeta de páginas](aspnet-mvc-4-dependency-injection/_static/image9.png "carpeta de páginas")

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample7.cs)]
4. Abra **Browse.cshtml** ver desde **/vistas/almacén** carpeta y hacer que se herede de **MyBasePage.cs**.

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample8.cshtml)]
5. En el **examinar** ver, agregar una llamada a **MessageService** para mostrar una imagen y un mensaje recuperado por el servicio.
   (C#)

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample9.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Including_a_Custom_Dependency_Resolver_and_a_Custom_View_Page_Activator"></a>
#### <a name="task-2---including-a-custom-dependency-resolver-and-a-custom-view-page-activator"></a>Tarea 2: incluyendo una resolución de dependencia personalizados y un activador de página de vista personalizada

En la tarea anterior, se insertado una dependencia nuevo dentro de una vista para realizar una llamada de servicio dentro de él. Ahora, se resolverá esa dependencia implementando las interfaces de inserción de dependencias de MVC de ASP.NET **IViewPageActivator** y **IDependencyResolver**. Se incluirá en la solución de una implementación de **IDependencyResolver** que abordará la recuperación de servicio mediante el uso de Unity. A continuación, se van a incluir otra implementación personalizada de **IViewPageActivator** interfaz que resolverá la creación de las vistas.

> [!NOTE]
> Desde ASP.NET MVC 3, la implementación para la inyección de dependencia tenía simplificado las interfaces para registrar los servicios. **Objeto IDependencyResolver** y **IViewPageActivator** forman parte de las características de ASP.NET MVC 3 para la inyección de dependencia.
> 
> **-IDependencyResolver** interfaz reemplaza el IMvcServiceLocator anterior. Los implementadores de IDependencyResolver deben devolver una instancia del servicio o una colección de servicio.
> 
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample10.cs)]
> 
> **-IViewPageActivator** interfaz proporciona un mayor control sobre cómo se crean instancias de páginas de vista a través de la inserción de dependencias. Las clases que implementan **IViewPageActivator** interfaz puede crear instancias de vista utilizando la información de contexto.
> 
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample11.cs)]


1. Crear la /**generadores** carpeta en la carpeta raíz del proyecto.
2. Incluir **CustomViewPageActivator.cs** a la solución de **/orígenes/activos/** a **generadores** carpeta. Para ello, haga clic en el **/Factories** carpeta, seleccione **agregar | Elemento existente** y, a continuación, seleccione **CustomViewPageActivator.cs**. Esta clase implementa la **IViewPageActivator** interfaz para albergar el contenedor de Unity.

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample12.cs)]

    > [!NOTE]
    > **CustomViewPageActivator** es responsable de administrar la creación de una vista mediante el uso de un contenedor de Unity.
3. Incluir **UnityDependencyResolver.cs** archivo **/orígenes/activos** a **/Factories** carpeta. Para ello, haga clic en el **/Factories** carpeta, seleccione **agregar | Elemento existente** y, a continuación, seleccione **UnityDependencyResolver.cs** archivo.

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample13.cs)]

    > [!NOTE]
    > **UnityDependencyResolver** clase es un DependencyResolver personalizado para Unity. Cuando un servicio no se encuentra dentro del contenedor de Unity, se invocated la resolución de base.

En la siguiente tarea ambas implementaciones se registrará para permitir que el modelo se conoce la ubicación de los servicios y las vistas.

<a id="Ex2Task3"></a>

<a id="Task_3_-_Registering_for_Dependency_Injection_within_Unity_container"></a>
#### <a name="task-3---registering-for-dependency-injection-within-unity-container"></a>Tarea 3: registrar para la inyección de dependencia en el contenedor de Unity

En esta tarea, se coloca todo lo anterior para que la inserción de dependencias funcione.

Hasta ahora la solución consta de los siguientes elementos:

- A **examinar** vista que hereda de **MyBaseClass** y consume **MessageService**.
- Una clase intermedia -**MyBaseClass**-que se declara para la interfaz de servicio de inserción de dependencias.
- Un servicio - **MessageService** - y su interfaz **IMessageService**.
- Una resolución de dependencia personalizados para Unity - **UnityDependencyResolver** -que abordan la recuperación de servicio.
- Un activador de página de vista - **CustomViewPageActivator** -que crea la página.

Insertar **examinar** vista, ahora se registrará la resolución de dependencia personalizados en el contenedor de Unity.

1. Abra **Bootstrapper.cs** archivo.
2. Registrar una instancia de **MessageService** en el contenedor de Unity para inicializar el servicio:

    (Código de fragmento de código: *servicio de mensajes de registro ASP.NET dependencia por inyección de código laboratorio - Ex02 -*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample14.cs)]
3. Agregue una referencia a **MvcMusicStore.Factories** espacio de nombres.

    (Código de fragmento de código: *Namespace de generadores ASP.NET dependencia inyección laboratorio - Ex02 -*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample15.cs)]
4. Registrar **CustomViewPageActivator** como un activador de página de vista en el contenedor de Unity:

    (Código de fragmento de código: *CustomViewPageActivator de registro ASP.NET dependencia inyección laboratorio - Ex02 -*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample16.cs)]
5. Reemplace la resolución de dependencia predeterminada de ASP.NET MVC 4 con una instancia de **UnityDependencyResolver**. Para ello, reemplace **Initialise** método contenido por el código siguiente:

    (Código de fragmento de código: *resolución de dependencias de actualización ASP.NET dependencia inyección laboratorio - Ex02 -*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample17.cs)]

    > [!NOTE]
    > ASP.NET MVC proporciona una clase de resolución de dependencia de manera predeterminada. Para trabajar con resoluciones de dependencia personalizados que hemos creado para unity, esta resolución debe reemplazarse.

<a id="Ex2Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Tarea 4: ejecutar la aplicación

En esta tarea, ejecutará la aplicación para comprobar que el Explorador de almacenamiento consume el servicio y se muestra la imagen y el mensaje recuperado:

1. Presione **F5** para ejecutar la aplicación.
2. Haga clic en **Rock** en el menú de géneros y vea cómo el **MessageService** insertado a la vista y cargar el mensaje de bienvenida y la imagen. En este ejemplo, estamos escriba con &quot; **Rock**&quot;:

    ![Tienda de música de MVC - vista inyección](aspnet-mvc-4-dependency-injection/_static/image10.png "tienda de música de MVC - inyección de vista")

    *Tienda de música de MVC - inyección de vista*
3. Cierre el explorador.

<a id="Exercise3"></a>

<a id="Exercise_3_Injecting_Action_Filters"></a>
### <a name="exercise-3-injecting-action-filters"></a>Ejercicio 3: Insertar filtros de acción

En la práctica anterior **filtros de acción personalizados** ha trabajado con inserción y personalización de filtros. En este ejercicio, obtendrá información sobre cómo insertar filtros con inyección de dependencia mediante el contenedor de Unity. Para ello, se agregará a la solución de la tienda de música un filtro de acción personalizada que se realizará un seguimiento la actividad del sitio.

<a id="Ex3Task1"></a>

<a id="Task_1_-_Including_the_Tracking_Filter_in_the_Solution"></a>
#### <a name="task-1---including-the-tracking-filter-in-the-solution"></a>Tarea 1: incluido el filtro de seguimiento de la solución

En esta tarea, se incluirá en la tienda de música un filtro de acción personalizado para eventos de seguimiento. Como filtro de acción personalizado ya se tratan conceptos en la práctica anterior &quot;filtros de acción personalizado&quot;, que solo incluya la clase de filtro de la carpeta de activos de este laboratorio y, a continuación, crear un proveedor de filtro para Unity:

1. Abra la **comenzar** soluciones se encuentran en la **Source\Ex03 - Filter\Begin de acción de insertar** carpeta. En caso contrario, puede seguir usando el **final** solución obtenido siguiendo el ejercicio anterior.

   1. Si abrió proporcionado **comenzar** solución, deberá descargar algunos paquetes de NuGet que faltan antes de continuar. Para ello, haga clic en el **proyecto** menú y seleccione **administrar paquetes de NuGet**.
   2. En el **administrar paquetes de NuGet** cuadro de diálogo, haga clic en **restaurar** para descargar los paquetes que falten.
   3. Por último, compile la solución haciendo clic en **generar** | **generar solución**.

      > [!NOTE]
      > Una de las ventajas del uso de NuGet es que no tiene que enviar todas las bibliotecas en el proyecto, lo que reduce el tamaño del proyecto. Con NuGet Power Tools, mediante la especificación de las versiones del paquete en el archivo Packages.config, podrá descargar la primera vez que ejecute el proyecto de todas las bibliotecas necesarias. Este es el motivo por el que se deben ejecutar estos pasos después de abrir una solución existente de este laboratorio.
      > 
      > Para obtener más información, consulte este artículo: [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages ](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).
2. Incluir **TraceActionFilter.cs** archivo **/orígenes/activos** a **/filtros** carpeta.

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample18.cs)]

    > [!NOTE]
    > Este filtro de acción personalizado realiza el seguimiento de ASP.NET. Puede comprobar &quot;local de ASP.NET MVC 4 y filtros de acción dinámicos&quot; laboratorio como referencia más.
3. Agregue la clase vacía **FilterProvider.cs** al proyecto en la carpeta   **/filtros.**
4. Agregar el **System.Web.Mvc** y **Microsoft.Practices.Unity** espacios de nombres en **FilterProvider.cs**.

    (Código de fragmento de código: *ASP.NET dependencia inyección laboratorio - Ex03 - el proveedor de filtros agregar espacios de nombres*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample19.cs)]
5. Haga que la clase herede de **IFilterProvider** interfaz.

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample20.cs)]
6. Agregar un **IUnityContainer** propiedad en el **FilterProvider** clase y, a continuación, cree un constructor de clase para asignar el contenedor.

    (Código de fragmento de código: *Constructor de proveedor de filtro ASP.NET dependencia inyección laboratorio - Ex03 -*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample21.cs)]

    > [!NOTE]
    > El constructor de clase de proveedor de filtro no está creando un **nuevo** dentro del objeto. El contenedor se pasa como un parámetro y se resuelve la dependencia de Unity.
7. En el **FilterProvider** clase, implemente el método **GetFilters** de **IFilterProvider** interfaz.

    (Código de fragmento de código: *GetFilters de proveedor de filtro ASP.NET dependencia inyección laboratorio - Ex03 -*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample22.cs)]

<a id="Ex3Task2"></a>

<a id="Task_2_-_Registering_and_Enabling_the_Filter"></a>
#### <a name="task-2---registering-and-enabling-the-filter"></a>Tarea 2: registrar y habilitar el filtro

En esta tarea, se habilita el seguimiento de sitio. Para ello, se registrará el filtro en **Bootstrapper.cs BuildUnityContainer** método para iniciar la traza:

1. Abra **Web.config** ubicado en el directorio raíz del proyecto y habilitar el seguimiento de traza en el grupo de System.Web.

    [!code-xml[Main](aspnet-mvc-4-dependency-injection/samples/sample23.xml)]
2. Abra **Bootstrapper.cs** en la raíz del proyecto.
3. Agregue una referencia a la **MvcMusicStore.Filters** espacio de nombres.

    (Código de fragmento de código: *agregar espacios de nombres ASP.NET dependencia inyección laboratorio - Ex03 - arranque*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample24.cs)]
4. Seleccione el **BuildUnityContainer** método y registrar el filtro en el contenedor de Unity. Tendrá que registrar el proveedor de filtros, así como el filtro de acción.

    (Código de fragmento de código: *ASP.NET dependencia inyección laboratorio - Ex03 - registro FilterProvider y ActionFilter*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample25.cs)]

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Tarea 3: ejecutar la aplicación

En esta tarea, ejecutará la aplicación y que el filtro de acción personalizada traza la actividad de prueba:

1. Presione **F5** para ejecutar la aplicación.
2. Haga clic en **Rock** en el menú de géneros. Puede examinar más géneros si desea.

    ![Tienda de música](aspnet-mvc-4-dependency-injection/_static/image11.png "tienda de música")

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

Al completar este laboratorio práctico ha aprendido cómo utilizar la inserción de dependencias en ASP.NET MVC 4 mediante la integración de Unity mediante un paquete de NuGet. Para lograrlo, ha utilizado la inyección de dependencia dentro de los controladores, vistas y filtros de acción.

Se tratan los conceptos siguientes:

- Características de inserción de dependencias de ASP.NET MVC 4
- Integración de Unity con paquete de NuGet Unity.Mvc3
- Inserción de dependencias en los controladores
- Inserción de dependencias en las vistas
- Inyección de dependencia de los filtros de acción

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Apéndice A: instalación de Visual Studio Express 2012 para Web

Puede instalar **Microsoft Visual Studio Express 2012 para Web** u otro &quot;Express&quot; versión usando la **[instalador de plataforma Web de Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)**. Las instrucciones siguientes le guían a través de los pasos necesarios para instalar *Visual studio Express 2012 para Web* con *instalador de plataforma Web de Microsoft*.

1. Vaya a [ [ https://go.microsoft.com/? linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). O bien, si ya ha instalado el instalador de plataforma Web, puede abrirla y busque el producto &quot; <em>Visual Studio Express 2012 for Web con SDK de Windows Azure</em>&quot;.
2. Haga clic en **instalar ahora**. Si no tiene **instalador de plataforma Web** se le redirigirá para descargarlo e instalarlo primero.
3. Una vez **instalador de plataforma Web** está abierto, haga clic en **instalar** para iniciar el programa de instalación.

    ![Instale Visual Studio Express](aspnet-mvc-4-dependency-injection/_static/image14.png "instale Visual Studio Express")

    *Instale Visual Studio Express*
4. Lea los términos y licencias de todos los productos y haga clic en **acepto** para continuar.

    ![Acepte los términos de licencia](aspnet-mvc-4-dependency-injection/_static/image15.png)

    *Acepte los términos de licencia*
5. Espere hasta que finalice el proceso de descarga e instalación.

    ![Progreso de la instalación](aspnet-mvc-4-dependency-injection/_static/image16.png)

    *Progreso de la instalación*
6. Cuando se complete la instalación, haga clic en **finalizar**.

    ![Se completó la instalación](aspnet-mvc-4-dependency-injection/_static/image17.png)

    *Se completó la instalación*
7. Haga clic en **Exit** para cerrar el instalador de plataforma Web.
8. Para abrir Visual Studio Express para Web, vaya a la **iniciar** pantalla y comienza a escribir &quot; **Express frente a**&quot;, a continuación, haga clic en el **VS Express para Web** colocar en mosaico.

    ![Express de VS para icono Web](aspnet-mvc-4-dependency-injection/_static/image18.png)

    *Express de VS para icono Web*

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a>Apéndice B: usar fragmentos de código

Con fragmentos de código, tiene todo el código que necesita a su alcance. El documento de laboratorio le indicará exactamente cuándo utilizarlas, tal como se muestra en la ilustración siguiente.

![Uso de fragmentos de código de Visual Studio para insertar código en el proyecto](aspnet-mvc-4-dependency-injection/_static/image19.png "fragmentos de código en Visual Studio para insertar código en el proyecto")

*Uso de fragmentos de código de Visual Studio para insertar código en el proyecto*

***Para agregar un fragmento de código mediante el teclado (solo C#)***

1. Coloque el cursor donde desea insertar el código.
2. Comience a escribir el nombre del fragmento (sin espacios ni guiones).
3. Observe como coincidencia de nombres de fragmentos de código de muestra de IntelliSense.
4. Seleccione el fragmento de código correcto (o siga escribiendo hasta que se selecciona el nombre del fragmento de código completo).
5. Presione la tecla Tab dos veces para insertar el fragmento de código en la ubicación del cursor.

![Comience a escribir el nombre del fragmento](aspnet-mvc-4-dependency-injection/_static/image20.png "comience a escribir el nombre del fragmento de código")

*Comience a escribir el nombre del fragmento de código*

![Presione la tecla Tab para seleccionar el fragmento de código resaltada](aspnet-mvc-4-dependency-injection/_static/image21.png "presione Tab para seleccionar el fragmento de código resaltada")

*Presione la tecla Tab para seleccionar el fragmento de código resaltada*

![Vuelva a presionar Tab y el fragmento de código se expandirán](aspnet-mvc-4-dependency-injection/_static/image22.png "vuelva a presionar Tab y el fragmento de código se expandirán")

*Vuelva a presionar Tab y el fragmento de código se expandirán*

***Para agregar un fragmento de código con el mouse (C#, en Visual Basic y XML)*** 1. Haga clic en donde desea insertar el fragmento de código.

1. Seleccione **Insertar fragmento de código** seguido **Mis fragmentos de código**.
2. Seleccione el fragmento de código relevante de la lista haciendo clic en él.

![Menú contextual donde desea insertar el fragmento de código y seleccione Insertar fragmento de código](aspnet-mvc-4-dependency-injection/_static/image23.png "contextual donde desea insertar el fragmento de código y seleccione Insertar fragmento de código")

*Haga clic en donde desea insertar el fragmento de código y seleccione Insertar fragmento de código*

![Seleccione el fragmento de código relevante de la lista haciendo clic en él](aspnet-mvc-4-dependency-injection/_static/image24.png "elegir el fragmento de código relevante de la lista haciendo clic en él")

*Seleccione el fragmento de código relevante de la lista haciendo clic en él*
